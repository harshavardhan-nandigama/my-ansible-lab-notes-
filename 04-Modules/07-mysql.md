

# Configure MySQL Component

## What this file does

This playbook automates the **installation and basic configuration of the MySQL database server**.
It uses a combination of Ansible modules to:

✅ Install MySQL server
✅ Start and enable MySQL service
✅ Secure the MySQL installation by setting root password

| Module    | Purpose                                |
| --------- | -------------------------------------- |
| `dnf`     | Install MySQL server package           |
| `service` | Start and enable MySQL service         |
| `command` | Run mysql\_secure\_installation script |

---

## Playbook Header Block

```yaml
- name: configure mysql
  hosts: mysql
  become: yes
```

---

## Explanation of key tasks

### 1️⃣ Install MySQL server

```yaml
- name: install
  ansible.builtin.dnf:
    name: mysql-server
    state: installed
```

**Module**: `dnf`
**Purpose**: Installs `mysql-server` package from OS repositories.

---

### 2️⃣ Start and enable MySQL service

```yaml
- name: start mysql server
  ansible.builtin.service:
    name: mysqld
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts MySQL server (`mysqld`) and ensures it is enabled to start on system boot.

---

### 3️⃣ Setup root password (secure installation)

```yaml
- name: setup root password
  ansible.builtin.command: mysql_secure_installation --set-root-pass RoboShop@1
```

**Module**: `command`
**Purpose**: Runs `mysql_secure_installation` with `--set-root-pass` option to set the MySQL root password (`RoboShop@1`).
This is part of the initial hardening process of a new MySQL installation.

---

## Why This Playbook Is Useful

* Automates the **installation of MySQL server** — no need to manually install and configure it.
* Ensures **MySQL is running** and set to start on boot.
* Automates the **secure configuration** of MySQL (root password setup), reducing the risk of human error.
* Saves time when setting up MySQL across multiple environments (Dev, QA, Production).

---

## Real-World Scenario

👉 In an eCommerce microservices app, components like **shipping** and **payment** typically use **MySQL** as their relational database.

👉 DevOps can use this playbook to:

✅ Quickly provision new **MySQL database servers**
✅ Automate **initial MySQL setup and security hardening**
✅ Ensure **consistent configuration** across environments
✅ Prepare databases for **application data** (tables, schemas, initial seed data can be added as next steps)

👉 This is very useful for **repeatable infrastructure deployments**, CI/CD pipelines, and even disaster recovery (where new DB servers must be provisioned rapidly).

