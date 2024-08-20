Per configurare password sudo diverse per ogni server nel tuo file `inventory.ini`, puoi utilizzare variabili specifiche per ogni host. Ecco come puoi strutturare il tuo file `inventory.ini` per gestire password sudo diverse per ogni server:

```ini
[servers]
client1 ansible_host=192.168.1.11 ansible_user=user1 ansible_become_pass=password1
client2 ansible_host=192.168.1.12 ansible_user=user2 ansible_become_pass=password2

[all:vars]
ansible_become=yes
ansible_become_method=sudo
```

In questo esempio:

1. `ansible_become_pass` è la variabile che specifica la password sudo per ogni host.
2. Ogni server ha la sua riga con le proprie specifiche variabili.
3. `[all:vars]` definisce variabili comuni a tutti gli host.

Se preferisci mantenere le password separate dal file di inventario (che è una pratica più sicura), puoi utilizzare file di variabili specifiche per ogni host. Ecco come:

1. Crea una directory `client_vars` nella stessa directory del tuo playbook.

2. Per ogni host, crea un file YAML con lo stesso nome dell'host. Ad esempio:

   ```
   client_vars/
   ├── client1.yml
   └── client2.yml
   ```

3. In ciascuno di questi file, inserisci le variabili specifiche per quell'host:

   **Contenuto di `client_vars/client1.yml`:**
   ```yaml
   ansible_become_pass: password1
   ```

   **Contenuto di `client_vars/client2.yml`:**
   ```yaml
   ansible_become_pass: password2
   ```

   E così via per ogni client.

4. Il tuo `inventory.ini` sarà più pulito:

   ```ini
   [servers]
   client1 ansible_host=192.168.1.11 ansible_user=user1
   client2 ansible_host=192.168.1.12 ansible_user=user2

   [all:vars]
   ansible_become=yes
   ansible_become_method=sudo
   ```

5. Lanciamo il comando che richiederà la password di vault:

   ```bash
   ansible-playbook -i inventory.ini playbook/playbook.yml --ask-vault-pass -vvv
   ```

Questo approccio offre diversi vantaggi:
- Migliore organizzazione
- Maggiore sicurezza (puoi escludere i file in `client_vars` dal controllo versione)
- Facilità di manutenzione

Ricorda che è sempre una buona pratica crittografare le password sensibili. Puoi farlo utilizzando Ansible Vault per ciascuno dei file in `client_vars`:

```bash
ansible-vault encrypt client_vars/client1.yml
```

In questo modo, le password saranno crittografate e dovrai fornire la password del vault quando esegui il playbook.
