
## Distribuzione di Apache sui Server con Ansible

Dopo aver creato un file `inventory.ini` contenente la lista dei server su cui vuoi distribuire Apache, segui questi passi per configurare e distribuire i pacchetti necessari sui server.

- Python3 su tutti i sistemi

### 1. Assicurati che i Server Siano Accessibili tramite SSH

Ansible comunica con i server utilizzando SSH. Verifica che:

- I server nel tuo inventario siano raggiungibili tramite SSH.
- Il tuo utente (spesso `root` o un utente con privilegi `sudo`) possa accedere a questi server senza richiedere una password, utilizzando le chiavi SSH.

### 2. Configurare la Chiave SSH

Se non hai già configurato l'accesso SSH con chiavi, segui questi passi:

1. **Genera una coppia di chiavi SSH** sul tuo computer locale:
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   Questo comando genera una chiave privata e una pubblica nella directory `~/.ssh/`.
   
2. **SSHD_CONFIG** nei server aggiungere riga vim /etc/sshd/sshd_config:

   ```bash
   PermitRootLogin yes
   ```
   e poi riavviamo sshd service:
   ```bash
   systemctl restart sshd
   ```
   oppure
   ```bash
   service sshd restart
   ```

3. **Copia la chiave pubblica sui server** nel tuo inventario:
   ```bash
   ssh-copy-id user@server_address
   ```
   Sostituisci `user` con l'utente che utilizzerai per connetterti (root) e `server_address` con l'indirizzo IP o il nome di dominio del server.

### 3. Esegui un Test di Connessione

Verifica che Ansible possa connettersi ai server nel tuo inventario:

```bash
ansible all -m ping -i inventary.ini
```

Se tutto è configurato correttamente, dovresti vedere una risposta `pong` da ciascun server.

### 4. Scrivere un Playbook per l'Installazione dei Pacchetti

Crea un playbook Ansible per installare i pacchetti necessari e configurare i server per eseguire il tuo script.

**Esempio di Playbook:**

```yaml
---
- hosts: servers
  become: yes
  become_method: su
  become_user: root  # L'utente con cui vuoi eseguire i comandi.

  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```

### 5. Eseguire il Playbook

Esegui il playbook per distribuire i pacchetti sui server del tuo inventario:

```bash
ansible-playbook -i inventory.ini playbook/playbook.yml -vvv
```

### 6. Verifica l'Installazione

Dopo aver eseguito il playbook, verifica che i pacchetti siano stati installati correttamente.
