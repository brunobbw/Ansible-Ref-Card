# Ansible Projekt

Dieses Projekt ist für das Modul 346 um einen Webserver und eine DB zu konfigurieren.

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
$ 
```
# test ob ansible funktioniert
```
$ ansible-inventory --graph
```
# test ob es sich mit maschine verbindet
```
$ ansible all -m ping
```
mit ansible -m ping db -> nur gruppe db
mit ansible -m ping web -> nur gruppe web

# Playbooks
testen:
```
$ ansible-playbook playbook.yml --check
```
ohne --check macht er wirklich also installieren
```
$ ansible-playbook playbook.yml
```

maven testen
```
$ 
```