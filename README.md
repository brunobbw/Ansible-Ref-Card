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

## Playbooks 
Testen:
```
$ ansible-playbook playbook.yml --check
```
ohne --check macht er wirklich also installieren
```
$ ansible-playbook playbook.yml
```
## Webserver Playbook

1. **Set Environment Variables**
   This task sets environment variables required by the Java application, such as `DB_USERNAME` and `DB_PASSWORD`.
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
````
2. **Install Maven**
   Installs the latest version of Maven on the web server.
     ```yaml
     - name: Install Maven
       ansible.builtin.apt:
          name: maven
          state: latest
          update_cache: yes
```
3. **Install Java**
   Installs the latest version of OpenJDK 11 on the web server.
    ```yaml
    - name: Install Java
      ansible.builtin.apt:
        name: openjdk-11-jdk
        state: latest
        update_cache: yes
```
4. **Clone Git Repository**
   Clones the specified Git repository into the directory `m346-ref-card-03`.
   ```yaml
   - name: Clone Git Repository
     ansible.builtin.git:
      repo: 'https://gitlab.com/bbwrl/m346-ref-card-03.git'
      dest: m346-ref-card-03
     tags:
      - clone

```
5. **Create Maven Package**
   Builds the Maven package for the Java application inside the `m346-ref-card-03` directory.
   ```yaml
   - name: Create Maven Package
     ansible.builtin.command:
       cmd: "mvn package"
       chdir: "m346-ref-card-03"
  tags:
    - maven

```
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
- **environment**: Tasks related to setting environment variables.
- **clone**: Task related to cloning the Git repository.
- **maven**: Tasks related to Maven, including package creation.
- **java**: Task related to executing the Java application.

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
