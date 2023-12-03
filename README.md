# Ref-Card-3-mit-Ansible-und-MaaS
Gruppenmitglieder: Bruno, Ismael
## Dokumentation
### Einleitung
Diese Aufgabe besteht darin zwei Maas Instanzen automatisiert zu installieren mithilfe von Ansible. 
- Ziel der Aufgabe:
  -  Zwei MaaS instanzen: Datenbank und Webserver
  -  JokesDB auf der Datenbank konfigurieren
  -  Schlussendlich soll eine Verbindung der beiden Instanzen erstellt werden, damit man Zugriff auf die Ref-Card-3.0 hat

## Struktur Ansible
Um das ganze Projekt zu erstellen, braucht man folgende Komponenten:
```
- ansible.cfg (um Konfigurationseinstellungen für Ansible zu definieren und anzupassen.)

- Cloud-init.yml (um die Instanzen mit definierte Kriterien (mit BBW-Maas) zu erstellen)

- hosts (um Hostnamen oder IP-Adressen von Zielsystemen zu definieren, auf denen Ansible-Aufgaben oder Playbooks ausgeführt werden sollen.)

- playbook.yml (automatisierte Aufgaben und Konfigurationsschritte für die Ausführung auf Zielsystemen.
)
```

# Getting started

Insallieren von Ansible
```
$ sudo apt update
$ sudo apt install ansible
```

Testen der Installation
```
$ ansible --version
```

SSH Key generieren
```
$ ssh-keygen
```
SSH Public Key anzeigen
```
$ cd /home/user/.ssh
$ cat id_rsa.pub
```
### Ansible testen
```
$ cd /pfad/zum/ansibleProjekt/
$ ansible-inventory --graph
```
Testen ob es sich mit der Maschine verbindet
```
$ ansible all -m ping
```
(Tipp beim Testen)
```
Mit: $ ansible -m ping db -> wird nur Gruppe db getestet
Mit: $ ansible -m ping web -> wird nur Gruppe web getestet
```

<<<<<<< HEAD
# ansible.cfg File erstellen
## Definition:
```
Ein "ansible.cfg"-Datei wird verwendet, um Konfigurationseinstellungen für Ansible zu definieren und anzupassen.
```
### Inhalt des Files
```
#ansible.cfg
[defaults]
inventory = hosts
```

# hosts File erstellen
## Definition:
```
Ein "hosts"-Datei in Ansible wird verwendet, um Hostnamen oder IP-Adressen von Zielsystemen zu definieren, auf denen Ansible-Aufgaben oder Playbooks ausgeführt werden sollen.
```
### Inhalt des Files
```
#hosts
[web]
web1 ansible_host=192.168.66.63 ansible_user=brunoismael

[db]
db1 ansible_host=192.168.66.8 ansible_user=brunoismael 
=======
## Playbooks 
Testen:
>>>>>>> 347e64569f99e4b57c49475b40ed0368586857c9
```

# cloud-init.yml File erstellen
## Definition:
```
Ein "cloud-init.yml"-Datei wird genutzt, um automatisierte Konfigurationen für virtuelle Maschinen in Cloud-Umgebungen festzulegen. 

Diese Datei enthält YAML-formatierte Anweisungen, die beim Starten einer VM durch das Cloud-Init-Tool interpretiert werden, um beispielsweise Benutzer, Netzwerkeinstellungen und Skriptausführungen zu konfigurieren.
```
### Inhalt des Files
```
#cloud-config
users:
  - default
  - name: userName
    sudo: ALL=(ALL) NOPASSWD:ALL
    shell: /bin/bash
    ssh_authorized_keys:
    - ssh-rsa deinPublicSSHKey...
```
## Webserver Playbook

<<<<<<< HEAD
# Playbooks erstellen
## Webserver
### Schritte: was alles in diesem Playbook kommt
#### Am Anfang des Playbooks kommt das hinein:
```
---
- name: Setup webserver
  hosts: web
  become: yes
```
#### Danach kommen die folgende tasks:
- Maven installieren
```
- name: Install maven
      ansible.builtin.apt:
        name: maven
        state: latest
        update_cache: yes
```
- Java installieren
```
- name: Install java
=======
1. **Set Environment Variables**
   Diese Aufgabe setzt die für die Java-Anwendung erforderlichen Umgebungsvariablen, wie `DB_USERNAME` und `DB_PASSWORD`.
   ```yaml 
   - name: Set environment variables for Java application
     ansible.builtin.shell:
       cmd: |
         echo "export DB_USERNAME=jokedbuser" >> /etc/environment
         echo "export DB_PASSWORD=123456" >> /etc/environment
         export DB_USERNAME=jokedbuser
         export DB_PASSWORD=123456
     tags:
       - environment
2. **Install Maven**
   Installiert die neueste Version von Maven auf dem Webserver.
     ```yaml
     - name: Install Maven
       ansible.builtin.apt:
          name: maven
          state: latest
          update_cache: yes

3. **Install Java**
   Installiert die neueste Version von OpenJDK 11 auf dem Webserver.
    ```yaml
    - name: Install Java
>>>>>>> 347e64569f99e4b57c49475b40ed0368586857c9
      ansible.builtin.apt:
        name: openjdk-11-jdk
        state: latest
        update_cache: yes
<<<<<<< HEAD
```
- Git-Repository klonen
```
- name: Git-Repository klonen
      ansible.builtin.git:
        repo: 'https://gitlab.com/bbwrl/m346-ref-card-01.git'
        dest: /home/userName/AuftragAnsible
```
- Maven-Paket erstellen
```
- name: Maven-Paket erstellen
      ansible.builtin.command:
        cmd: mvn package
        chdir: /home/userName/AuftragAnsible/m346-ref-card-01
```
- JAR ausführen
```
- name: JAR ausführen
      ansible.builtin.command:
        cmd: java -DDB_USERNAME="jokedbuser" -DDB_PASSWORD="123456" -jar target/architecture-refcard-03-0.0.1-SNAPSHOT.jar
```


## Database
### Schritte: was alles in diesem Playbook kommt
#### Am Anfang des Playbooks kommt das hinein:
```
---
- name: Setup database
  hosts: db
  become: yes
```
#### Danach kommen die folgende tasks:
- Docker installieren
```
- name: Install Docker
      apt:
        name: docker.io
        state: present
```
- Docker Compose installieren
```
- name: Install Docker Compose
      apt:
        name: docker-compose
        state: present
```
- Git-Repository klonen
```
- name: Git-Repository klonen
      ansible.builtin.git:
        repo: 'https://gitlab.com/bbwrl/m346-ref-card-03/'
        dest: m346-ref-card-03
      tags:
        - clone
```
- Docker-Compose File starten
```
- name: Start Docker Compose
      command: "docker-compose -f m346-ref-card-03/docker-compose.yml up -d"
```

### Playbooks ausführen
1. Im richtigen Verzeichnis wechseln (wo die Playbooks sind)
```
cd /pfad/zum/projekt
```
2. Playbooks testen
```
$ ansible-playbook webserver-playbook.yml --check
$ ansible-playbook database-playbook.yml --check
```
2. Playbooks ausführen
```
$ ansible-playbook webserver-playbook.yml
$ ansible-playbook database-playbook.yml 
```

## Schlussfolgerung
**Das Ansible-Projekt war ein Erfolg!**
**Wir haben mithilfe von MAAS zwei Instanzen für Datenbank und Webserver erstellt und mit Ansible die gesamte Infrastruktur effizient konfiguriert.**

**Trotz Herausforderungen, vor allem bei der Verbindung zwischen den beiden Instanzen, geling uns diese Arbeit gut.**

**Der Einsatz von Automatisierungstools wie Ansible sparte nicht nur Zeit, sondern verbesserte auch die Konsistenz unserer Infrastruktur. Dieses Projekt war eine lehrreiche Erfahrung für die Anwendung von Cloud-Technologien und Automatisierung in der Praxis.**
=======

4. **Clone Git Repository**
  Klonen des angegebenen Git-Repository in das Verzeichnis m346-ref-card-03.
  ```yaml
   - name: Clone Git Repository
     ansible.builtin.git:
      repo: 'https://gitlab.com/bbwrl/m346-ref-card-03.git'
      dest: m346-ref-card-03
     tags:
      - clone


6. **Create Maven Package**
   Erstellt das Maven-Paket für die Java-Anwendung im Verzeichnis m346-ref-card-03.
   ```yaml
   - name: Create Maven Package
     ansible.builtin.command:
       cmd: "mvn package"
       chdir: "m346-ref-card-03"
  tags:
    - maven


6. **Execute JAR**
   Runs the Java application using the generated JAR file, setting the previously defined environment variables.
   ```yaml
   - name: Execute JAR
     ansible.builtin.command:
       cmd: "java -DDB_USERNAME=jokedbuser -DDB_PASSWORD=123456 -jar m346-ref-card-03/target/architecture-refcard-03-0.0.1-SNAPSHOT.jar &"
   args:
      executable: /bin/bash
   tags:
      - java


**Tags**
- **environment**: Aufgaben im Zusammenhang mit dem Setzen von Umgebungsvariablen.
- **clone**: Aufgabe im Zusammenhang mit dem Klonen des Git-Repository.
- **maven**: Aufgaben im Zusammenhang mit Maven, einschließlich Paketerstellung.
- **java**: Aufgabe im Zusammenhang mit der Ausführung der Java-Anwendung.

## Datenbank Playbook
### Installationen
=======
$ Wird noch definiert
```
>>>>>>> d2813122a556af14ffea7d191d63d7cc2cd06104


maven testen
```
<<<<<<< HEAD
$   
```
>>>>>>> 347e64569f99e4b57c49475b40ed0368586857c9
