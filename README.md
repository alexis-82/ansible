
# Ansible

**Ansible** è uno strumento open-source per l'automazione IT che consente di gestire la configurazione, il deployment e l'orchestrazione di applicazioni su larga scala. È particolarmente apprezzato per la sua semplicità e facilità d'uso, grazie all'assenza di agenti software da installare sui nodi gestiti.

## Caratteristiche Principali

- **Senza agenti**: Ansible non richiede l'installazione di agenti sui nodi remoti. Utilizza SSH per la comunicazione.
- **Semplicità**: Le configurazioni sono scritte in YAML, un linguaggio leggibile e facile da comprendere.
- **Modularità**: Ansible offre una vasta gamma di moduli per gestire operazioni diverse come il provisioning, la configurazione di servizi, e la gestione di utenti.
- **Scalabilità**: Ansible può gestire da pochi server fino a intere infrastrutture composte da migliaia di nodi.
- **Idempotenza**: Assicura che le operazioni siano eseguite solo se necessario, mantenendo lo stato desiderato del sistema.

## Utilizzo Tipico

- **Provisioning**: Configurazione iniziale di server e ambienti.
- **Configurazione**: Gestione delle configurazioni dei servizi e delle applicazioni.
- **Deploy**: Automazione dei processi di rilascio e aggiornamento delle applicazioni.
- **Orchestrazione**: Coordinazione di processi multipli su sistemi distribuiti.

## Esempio di Playbook

Ecco un esempio semplice di un playbook Ansible che installa Apache su un server:

```yaml
---
- hosts: webservers
  become: yes # predefinito è sudo

  tasks:
  - name: Install Apache
    apt:
      name: apache2
      state: present
```

Con Ansible, l'automazione IT diventa più gestibile, scalabile e meno soggetta a errori, semplificando la vita di amministratori di sistema e ingegneri DevOps.