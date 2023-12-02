# Ref-Card-3-mit-Ansible-und-MaaS
Gruppenmitglieder: Bruno, Ismael
## Dokumentation
### Einleitung
Diese Aufgabe besteht darin zwei Maas Instanzen automatisiert zu installieren mit der Hilfe von Ansible. 
- Ziel der Aufgabe:
  -  Zwei MaaS instanzen: Datenbank und Webserver
  -  JokesDb konfigurieren auf der Datenbank
  -  Schlussendlich soll eine Verbindung der beiden Instanzen erstellt werden, damit man Zugriff auf die ReF-Card 3.0 hat


## Getting started

Insallieren von Ansible
```
$ sudo apt update
$ sudo apt install ansible
```

Testen der Installation
```
$ ansible --version
```

## Struktur Ansible

```
ansible.cfg     Konfiguration für Ansible, hauptsächlich Ort des Inventories

Cloud-init.yml      Cloud-Init für BBW-Maas
```
SSH Public Key anzeigen
```
$ cd /home/user/.ssh
$ cat id_rsa.pub
```
## Test ob ansible funktioniert
```
$ ansible-inventory --graph
```
## Test ob es sich mit maschine verbindet
```
$ ansible all -m ping
```
mit ansible -m ping db -> nur gruppe db
mit ansible -m ping web -> nur gruppe web

# Playbooks 

## Webserver

Testen:
```
$ ansible-playbook playbook.yml --check
```
ohne --check macht er wirklich also installieren
```
$ ansible-playbook playbook.yml
```

maven testen
```
$ Wird noch definiert
```
