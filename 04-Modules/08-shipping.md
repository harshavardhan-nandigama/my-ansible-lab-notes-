

#  Configure Shipping Component

## What this file does

This playbook automates the **deployment and configuration of the `shipping` component** in a microservices-based eCommerce application.

It uses a combination of Ansible modules to:

✅ Install required software (Maven, MySQL client, Python packages)
✅ Prepare the application environment
✅ Download and deploy the Shipping Java app
✅ Build the application using Maven
✅ Configure and start the Shipping service
✅ Import necessary data into MySQL database

| Module                     | Purpose                                         |
| -------------------------- | ----------------------------------------------- |
| `dnf`                      | Install Maven, MySQL client                     |
| `pip`                      | Install Python packages (pymysql, cryptography) |
| `file`                     | Create application directory                    |
| `user`                     | Create system user                              |
| `get_url`                  | Download application zip                        |
| `unarchive`                | Extract application zip                         |
| `command`                  | Build app with Maven, rename jar                |
| `copy`                     | Copy service file                               |
| `systemd_service`          | Reload systemd daemon                           |
| `service`                  | Start and enable service                        |
| `community.mysql.mysql_db` | Import database schema and data                 |

---

## Playbook Header Block

```yaml
- name: configure shipping component
  hosts: shipping
  become: yes
```

---

## Explanation of key tasks

### 1️⃣ Install Maven and MySQL client

```yaml
- name: install maven and mysql
  ansible.builtin.dnf:
    name: "{{ item }}"
    state: present
  loop:
    - maven
    - mysql
```

**Module**: `dnf`
**Purpose**: Installs both **Maven** (for building the Java app) and **MySQL client**.

---

### 2️⃣ Install pymysql and cryptography (Python dependencies)

```yaml
- name: install pymysql and cryptography
  ansible.builtin.pip:
    name: "{{ item }}"
    executable: pip3.9
  loop:
    - cryptography
    - pymysql
```

**Module**: `pip`
**Purpose**: Installs required Python modules for interacting with MySQL from Ansible.

---

### 3️⃣ Create application directory

```yaml
- name: create app directory
  ansible.builtin.file:
    path: /app
    state: directory
```

**Module**: `file`
**Purpose**: Creates `/app` directory to host the Shipping component code.

---

### 4️⃣ Create roboshop system user

```yaml
- name: create roboshop system user
  ansible.builtin.user:
    name: roboshop
    shell: /sbin/nologin
    system: true
    home: /app
```

**Module**: `user`
**Purpose**: Creates a system user `roboshop` under `/app` with no login shell.

---

### 5️⃣ Download Shipping code

```yaml
- name: download shipping code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/shipping-v3.zip
    dest: /tmp/shipping.zip
```

**Module**: `get_url`
**Purpose**: Downloads the latest Shipping application code (zip file).

---

### 6️⃣ Extract Shipping code

```yaml
- name: extract shipping code
  ansible.builtin.unarchive:
    src: /tmp/shipping.zip
    dest: /app
    remote_src: yes
```

**Module**: `unarchive`
**Purpose**: Extracts the Shipping code into `/app`.

---

### 7️⃣ Build the application using Maven

```yaml
- name: install maven dependencies
  ansible.builtin.command: mvn clean package
  args:
    chdir: /app
```

**Module**: `command`
**Purpose**: Runs `mvn clean package` to build the Shipping Java app into a JAR.

---

### 8️⃣ Rename JAR file

```yaml
- name: rename jar file
  ansible.builtin.command: mv target/shipping-1.0.jar shipping.jar 
  args:
    chdir: /app
```

**Module**: `command`
**Purpose**: Renames the generated JAR file to `shipping.jar` for consistent usage.

---

### 9️⃣ Copy Shipping service file

```yaml
- name: copy shipping service
  ansible.builtin.copy:
    src: shipping.service
    dest: /etc/systemd/system/shipping.service
```

**Module**: `copy`
**Purpose**: Deploys the systemd unit file for the Shipping service.

---

### 🔟 Reload systemd daemon

```yaml
- name: daemon reload
  ansible.builtin.systemd_service:
    daemon_reload: true
```

**Module**: `systemd_service`
**Purpose**: Reloads systemd daemon to recognize new/updated service.

---

### 1️⃣1️⃣ Start and enable Shipping service

```yaml
- name: enable and start shipping
  ansible.builtin.service:
    name: shipping
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts the Shipping service and enables it on boot.

---

### 1️⃣2️⃣ Import MySQL data

```yaml
- name: import data
  tags:
    - import
  community.mysql.mysql_db:
    name: all
    login_user: root
    login_password: RoboShop@1
    login_host: mysql.harshavn24.site
    state: import
    target: "{{ item }}"
  loop:
    - /app/db/schema.sql
    - /app/db/app-user.sql
    - /app/db/master-data.sql
```

**Module**: `community.mysql.mysql_db`
**Purpose**: Imports the application schema and data into MySQL:

✅ **schema.sql** — DB schema
✅ **app-user.sql** — Application users
✅ **master-data.sql** — Initial data for the app

---

### 1️⃣3️⃣ Restart Shipping service (after DB import)

```yaml
- name: restart shipping
  tags: 
    - import
  ansible.builtin.service:
    state: restarted
    name: shipping
```

**Module**: `service`
**Purpose**: Restarts the Shipping service so that it picks up any configuration or data changes.

---

## Why This Playbook Is Useful

* Automates the complete deployment of a **Java-based microservice** (Shipping component).
* Handles **application build** (using Maven) and deployment.
* Ensures **database schema and data** are initialized correctly in MySQL.
* Provides a **repeatable and consistent** deployment process across environments (Dev, QA, Prod).
* Makes it easy for new team members to set up the Shipping service without manual steps.

---

## Real-World Scenario

👉 The **Shipping component** is a Java service that relies on **MySQL** for data storage.
👉 It is a critical part of an **eCommerce app** for managing shipment-related logic.

With this playbook, DevOps engineers can:

✅ Provision new Shipping service instances quickly.
✅ Automate initial database setup — important for **disaster recovery** and **CI/CD pipelines**.
✅ Rebuild or update the service safely whenever the application code changes.
✅ Maintain consistency across multiple environments.

