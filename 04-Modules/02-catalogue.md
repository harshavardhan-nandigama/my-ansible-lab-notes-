# Playbook: Configure Catalogue Component

## What this file does

This playbook automates the **deployment and configuration of the `catalogue` component** of an eCommerce application.  
It uses a combination of Ansible modules to:

✅ Manage Node.js version  
✅ Set up application directory and user  
✅ Download and deploy application code  
✅ Install dependencies  
✅ Configure and start the catalogue service  
✅ Configure MongoDB client and load initial data  


| Module                  | Purpose                                           |
| ----------------------- | ------------------------------------------------- |
| `command`               | Run shell command (disable/enable Node.js module) |
| `dnf`                   | Install Node.js and MongoDB client                |
| `file`                  | Create application directory                      |
| `user`                  | Create system user                                |
| `get_url`               | Download application zip                          |
| `unarchive`             | Extract application zip                           |
| `community.general.npm` | Install Node.js dependencies                      |
| `copy`                  | Copy service and repo files                       |
| `systemd`               | Reload systemd daemon                             |
| `service`               | Start and enable service                          |
| `debug`                 | Print debug output                                |
| `shell`                 | Run MongoDB shell to load data                    |

## Playbook Header Block


    - name: configure catalogue component
      hosts: catalogue
      become: yes

## Explanation of key tasks

### 1️⃣ Disable default Node.js

        - name: disable default nodejs
          ansible.builtin.command: dnf module disable nodejs -y


Module: command

Purpose: Disables default Node.js module to avoid version conflict.

### 2️⃣ Enable Node.js 20

    - name: enable nodejs:20
      ansible.builtin.command: dnf module enable nodejs:20 -y

Module: command

Purpose: Enables Node.js version 20 module.

### 3️⃣ Install Node.js

    - name: install nodejs
      ansible.builtin.dnf:
        name: nodejs
        state: present

Module: dnf

Purpose: Installs Node.js from enabled module stream.

### 4️⃣ Create application directory

- name: create app directory
  ansible.builtin.file:
    path: /app
    state: directory

Module: file

Purpose: Creates /app directory to host application code.

### 5️⃣ Create roboshop system user

    - name: create roboshop system user
      ansible.builtin.user:
        name: roboshop
        shell: /sbin/nologin
        system: true
        home: /app

Module: user

Purpose: Creates roboshop system user with /app as home, no shell login.

### 6️⃣ Download catalogue code

    - name: download catalogue code
      ansible.builtin.get_url:
        url: https://roboshop-artifacts.s3.amazonaws.com/catalogue-v3.zip
        dest: /tmp/catalogue.zip

Module: get_url

Purpose: Downloads the application zip from S3.

### 7️⃣ Extract catalogue code

    - name: extract catalogue code
      ansible.builtin.unarchive:
        src: /tmp/catalogue.zip
        dest: /app
        remote_src: yes

Module: unarchive

Purpose: Extracts application code to /app.

### 8️⃣ Install Node.js dependencies

- name: install dependencies
  community.general.npm:
    path: /app

Module: community.general.npm

Purpose: Installs Node.js dependencies (package.json) in /app.

### 9️⃣ Copy catalogue service file

    - name: copy catalogue service to system directory
      ansible.builtin.copy:
        src: catalogue.service
        dest: /etc/systemd/system/catalogue.service

Module: copy

Purpose: Deploys systemd service unit file for catalogue service.

### 10️⃣ Reload systemd daemon

    - name: systemctl daemon reload
      ansible.builtin.systemd:
        daemon_reload: yes

Module: systemd

Purpose: Reloads systemd to pick up new service file.

### 11️⃣ Start and enable catalogue service

    - name: start and enable catalogue
      ansible.builtin.service:
        name: catalogue
        state: started
        enabled: yes
        
Module: service

Purpose: Starts and enables catalogue service.

### 12️⃣ Copy MongoDB repo

    - name: copy mongodb repo
      ansible.builtin.copy:
        src: mongo.repo
        dest: /etc/yum.repos.d/mongo.repo

Module: copy

Purpose: Copies MongoDB repo file to configure package source.


### 13️⃣ Install MongoDB client

    - name: install mongodb client
      ansible.builtin.dnf:
        name: mongodb-mongosh
        state: present

Module: dnf

Purpose: Installs mongosh CLI tool to interact with MongoDB.

### 14️⃣ Check if products loaded

    - name: check products loaded or not
      ansible.builtin.command: mongosh --host mongodb.harshavn24.site --eval 'db.getMongo().getDBNames().indexOf("catalogue")'
      register: catalogue_output
        
Module: command

Purpose: Runs MongoDB shell command to check if catalogue database exists.

Registers result in catalogue_output.

### 15️⃣ Print catalogue output

    - name: print catalogue output
      ansible.builtin.debug:
        msg: "{{ catalogue_output.stdout }}"

Module: debug

Purpose: Prints the value of catalogue_output.stdout for visibility.

### 16️⃣ Load products (conditionally)

    - name: load products
      ansible.builtin.shell: mongosh --host mongodb.harshavn24.site < /app/db/master-data.js
      when: catalogue_output.stdout | int < 0
Module: shell

Purpose: Loads master-data.js into MongoDB only if catalogue database is not present (index < 0).

Conditional execution using when.


### Summary
This is an excellent example of combining many useful Ansible modules:

✅ command, dnf, file, user, get_url, unarchive, community.general.npm, copy, systemd, service, debug, shell, replace (in earlier MongoDB playbook).

- Shows good practical real-world automation:

- Software installation

- App deployment

- Service management

- MongoDB data initialization

- Use of conditions and debug output