

#  Configure User Component

## What this file does

This playbook automates the **deployment and configuration of the `user` component** of an eCommerce application.
It covers:

✅ Managing Node.js version modules
✅ Creating system user and app directory
✅ Downloading and deploying the user application code
✅ Installing Node.js dependencies
✅ Configuring and starting the user systemd service

| Module                  | Purpose                       |
| ----------------------- | ----------------------------- |
| `command`               | Disable/Enable Node.js module |
| `dnf`                   | Install Node.js package       |
| `user`                  | Create system user            |
| `file`                  | Create application directory  |
| `get_url`               | Download application archive  |
| `unarchive`             | Extract application archive   |
| `community.general.npm` | Install Node.js dependencies  |
| `copy`                  | Copy systemd service file     |
| `systemd`               | Reload systemd daemon         |
| `service`               | Start and enable user service |

---

## Playbook Header Block

```yaml
- name: configure user component
  hosts: user
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
**Purpose**: Disables the default Node.js module to avoid version conflicts.

---

### 2️⃣ Enable Node.js 20 module

```yaml
- name: enable nodejs:20
  ansible.builtin.command: dnf module enable nodejs:20 -y
```

**Module**: `command`
**Purpose**: Enables Node.js version 20 module stream.

---

### 3️⃣ Install Node.js

```yaml
- name: install nodejs
  ansible.builtin.dnf:
    name: nodejs
    state: present
```

**Module**: `dnf`
**Purpose**: Installs Node.js package on the system.

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
**Purpose**: Creates a system user `roboshop` with no login shell and home at `/app`.

---

### 5️⃣ Create application directory

```yaml
- name: create app directory
  ansible.builtin.file:
    path: /app
    state: directory
```

**Module**: `file`
**Purpose**: Creates the `/app` directory to host application files.

---

### 6️⃣ Download user code

```yaml
- name: download user code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/user-v3.zip
    dest: /tmp/user.zip
```

**Module**: `get_url`
**Purpose**: Downloads the user component zip archive from the S3 bucket.

---

### 7️⃣ Extract user code

```yaml
- name: extract user code
  ansible.builtin.unarchive:
    src: /tmp/user.zip
    dest: /app
    remote_src: yes
```

**Module**: `unarchive`
**Purpose**: Extracts the downloaded zip into `/app`.

---

### 8️⃣ Install Node.js dependencies

```yaml
- name: install dependencies
  community.general.npm:
    path: /app
```

**Module**: `community.general.npm`
**Purpose**: Installs Node.js dependencies using npm in `/app`.

---

### 9️⃣ Copy user systemd service file

```yaml
- name: copy user service to system directory
  ansible.builtin.copy:
    src: user.service
    dest: /etc/systemd/system/user.service
```

**Module**: `copy`
**Purpose**: Copies the user service systemd unit file to the system directory.

---

### 🔟 Reload systemd daemon

```yaml
- name: reload systemd to pick up new service file
  ansible.builtin.systemd:
    daemon_reload: true
```

**Module**: `systemd`
**Purpose**: Reloads systemd to recognize the new or changed service file.

---

### 1️⃣1️⃣ Start and enable user service

```yaml
- name: start and enable user
  ansible.builtin.service:
    name: user
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts the user service and enables it to start automatically on boot.

---

## Why This Playbook Is Useful

* Automates consistent deployment of the user microservice on multiple servers.
* Manages Node.js version and dependencies cleanly.
* Sets up the correct system user and directories for security and organization.
* Automates service creation and lifecycle management.
* Reduces manual intervention and deployment errors.

---

## Real-World Scenario

👉 In a microservices-based eCommerce application:

* The user service handles user registration, login, and profile management.
* Needs the correct Node.js environment and dependencies to run smoothly.
* This playbook lets DevOps engineers quickly deploy or redeploy the user service during updates or scaling.
* Ensures the service is running and enabled after reboot automatically.
* Facilitates a repeatable deployment process in CI/CD pipelines or multi-node environments.

