
# Configure Catalogue Component

## What this file does

This playbook automates the **deployment and configuration of the `catalogue` component** of an eCommerce application.
It manages:

✅ Node.js version and installation
✅ App directory and system user setup
✅ Downloading and extracting the catalogue application code
✅ Installing Node.js dependencies
✅ Setting up and managing the catalogue systemd service
✅ Installing MongoDB client and configuring repo
✅ Checking and loading product data into MongoDB only if needed

| Module                  | Purpose                                         |
| ----------------------- | ----------------------------------------------- |
| `command`               | Disable/enable Node.js module, check MongoDB db |
| `dnf`                   | Install Node.js and MongoDB client              |
| `file`                  | Create app directory                            |
| `user`                  | Create roboshop system user                     |
| `get_url`               | Download catalogue code zip                     |
| `unarchive`             | Extract code zip                                |
| `community.general.npm` | Install Node.js dependencies                    |
| `copy`                  | Copy service file and MongoDB repo              |
| `systemd`               | Reload systemd daemon                           |
| `service`               | Start and enable catalogue service              |
| `debug`                 | Print command output                            |
| `shell`                 | Load MongoDB data conditionally                 |

---

## Playbook Header Block

```yaml
- name: configure catalogue component
  hosts: catalogue
  become: yes
```

---

## Explanation of key tasks

### 1️⃣ Disable default Node.js module

```yaml
- name: disable default nodejs
  ansible.builtin.command: dnf module disable nodejs -y
```

**Module**: `command`
**Purpose**: Disables the default Node.js module stream to prevent conflicts.

---

### 2️⃣ Enable Node.js 20 module

```yaml
- name: enable nodejs:20
  ansible.builtin.command: dnf module enable nodejs:20 -y
```

**Module**: `command`
**Purpose**: Enables Node.js version 20 module stream for installation.

---

### 3️⃣ Install Node.js

```yaml
- name: install nodejs
  ansible.builtin.dnf:
    name: nodejs
    state: present
```

**Module**: `dnf`
**Purpose**: Installs Node.js package from the enabled module stream.

---

### 4️⃣ Create application directory

```yaml
- name: create app directory
  ansible.builtin.file:
    path: /app
    state: directory
```

**Module**: `file`
**Purpose**: Creates the `/app` directory for the application code.

---

### 5️⃣ Create roboshop system user

```yaml
- name: create roboshop system user
  ansible.builtin.user:
    name: roboshop
    shell: /sbin/nologin
    system: true
    home: /app
```

**Module**: `user`
**Purpose**: Creates the `roboshop` system user with no login shell and `/app` as home directory.

---

### 6️⃣ Download catalogue application code

```yaml
- name: download catalogue code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/catalogue-v3.zip
    dest: /tmp/catalogue.zip
```

**Module**: `get_url`
**Purpose**: Downloads the catalogue component archive from S3.

---

### 7️⃣ Extract catalogue code

```yaml
- name: extract catalogue code
  ansible.builtin.unarchive:
    src: /tmp/catalogue.zip
    dest: /app
    remote_src: yes
```

**Module**: `unarchive`
**Purpose**: Extracts the downloaded archive into `/app`.

---

### 8️⃣ Install Node.js dependencies

```yaml
- name: install dependencies
  community.general.npm:
    path: /app
```

**Module**: `community.general.npm`
**Purpose**: Installs the required Node.js packages inside `/app`.

---

### 9️⃣ Copy catalogue systemd service file

```yaml
- name: copy catalogue service to system directory
  ansible.builtin.copy:
    src: catalogue.service
    dest: /etc/systemd/system/catalogue.service
```

**Module**: `copy`
**Purpose**: Copies the systemd service definition for the catalogue service.

---

### 🔟 Reload systemd daemon

```yaml
- name: systemctl daemon reload
  ansible.builtin.systemd:
    daemon_reload: yes
```

**Module**: `systemd`
**Purpose**: Reloads systemd manager configuration to recognize new service files.

---

### 1️⃣1️⃣ Start and enable catalogue service

```yaml
- name: start and enable catalogue
  ansible.builtin.service:
    name: catalogue
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts and enables the catalogue service to run on boot.

---

### 1️⃣2️⃣ Copy MongoDB repository file

```yaml
- name: copy mongodb repo
  ansible.builtin.copy:
    src: mongo.repo
    dest: /etc/yum.repos.d/mongo.repo
```

**Module**: `copy`
**Purpose**: Adds MongoDB yum repo config to install MongoDB tools.

---

### 1️⃣3️⃣ Install MongoDB client (mongosh)

```yaml
- name: install mongodb client
  ansible.builtin.dnf:
    name: mongodb-mongosh
    state: present
```

**Module**: `dnf`
**Purpose**: Installs `mongosh`, the MongoDB shell client.

---

### 1️⃣4️⃣ Check if catalogue products data is loaded

```yaml
- name: check products loaded or not
  ansible.builtin.command: mongosh --host mongodb.harshavn24.site --eval 'db.getMongo().getDBNames().indexOf("catalogue")'
  register: catalogue_output
```

**Module**: `command`
**Purpose**: Runs a MongoDB shell command to check if the `catalogue` database exists.

---

### 1️⃣5️⃣ Debug print catalogue output

```yaml
- name: print catalogue output
  ansible.builtin.debug:
    msg: "{{ catalogue_output.stdout }}"
```

**Module**: `debug`
**Purpose**: Outputs the command result for verification/logging.

---

### 1️⃣6️⃣ Load catalogue products data if missing

```yaml
- name: load products
  ansible.builtin.shell: mongosh --host mongodb.harshavn24.site < /app/db/master-data.js
  when: catalogue_output.stdout | int < 0
```

**Module**: `shell`
**Purpose**: Runs a script to load initial product data into MongoDB only if the `catalogue` DB does not exist (`indexOf` returns -1).

---

## Why This Playbook Is Useful

* Ensures the catalogue microservice runs on the correct Node.js version.
* Automates setup of the service environment and code deployment.
* Installs MongoDB client tools and configures repository for package management.
* Checks and conditionally loads initial product data, preventing duplication.
* Handles service lifecycle cleanly with systemd.
* Provides idempotent, repeatable deployment for production environments.

---

## Real-World Scenario

👉 For a microservices eCommerce app:

* The catalogue service holds product data and APIs.
* Requires Node.js v20 and MongoDB connectivity.
* This playbook automates deployment on multiple nodes or during updates.
* It safeguards against reloading data unnecessarily, keeping the database clean.
* Supports continuous integration pipelines and scaling by automating the entire lifecycle.

---
