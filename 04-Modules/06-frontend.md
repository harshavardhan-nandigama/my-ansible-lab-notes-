

#  Configure Frontend Component

## What this file does

This playbook automates the **deployment and configuration of the Frontend component** of an eCommerce application.
It configures **NGINX** as a web server to serve static frontend content.

✅ Manages NGINX version
✅ Installs NGINX
✅ Deploys frontend static content
✅ Configures NGINX with custom config
✅ Restarts NGINX to serve new content

| Module      | Purpose                                |
| ----------- | -------------------------------------- |
| `command`   | Enable/disable nginx module            |
| `dnf`       | Install nginx                          |
| `service`   | Start, enable, restart nginx service   |
| `file`      | Manage html directory and config files |
| `get_url`   | Download frontend zip                  |
| `unarchive` | Extract frontend zip                   |
| `copy`      | Deploy custom nginx configuration      |

---

## Playbook Header Block

```yaml
- name: configuring frontend component
  hosts: frontend
  become: yes
```

---

## Explanation of key tasks

### 1️⃣ Disable default nginx

```yaml
- name: disable default nginx
  ansible.builtin.command: dnf module disable nginx -y
```

**Module**: `command`
**Purpose**: Disables the default nginx module to avoid version conflict.

---

### 2️⃣ Enable nginx 1.24

```yaml
- name: enable nginx
  ansible.builtin.command: dnf module enable nginx:1.24 -y
```

**Module**: `command`
**Purpose**: Enables nginx version 1.24 stream.

---

### 3️⃣ Install nginx

```yaml
- name: install nginx
  ansible.builtin.dnf:
    name: nginx
    state: present
```

**Module**: `dnf`
**Purpose**: Installs nginx package.

---

### 4️⃣ Start and enable nginx service

```yaml
- name: start and enable nginx
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts nginx and enables it to run at boot.

---

### 5️⃣ Remove default html directory

```yaml
- name: remove html directory
  ansible.builtin.file:
    path: /usr/share/nginx/html
    state: absent
```

**Module**: `file`
**Purpose**: Removes default html directory to avoid conflict with new content.

---

### 6️⃣ Create html directory

```yaml
- name: create html directory
  ansible.builtin.file:
    path: /usr/share/nginx/html
    state: directory
```

**Module**: `file`
**Purpose**: Creates html directory where frontend content will be deployed.

---

### 7️⃣ Download frontend code

```yaml
- name: download frontend code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/frontend-v3.zip
    dest: /tmp/frontend.zip
```

**Module**: `get_url`
**Purpose**: Downloads frontend static content zip from S3.

---

### 8️⃣ Extract frontend code

```yaml
- name: unzip frontend code
  ansible.builtin.unarchive:
    src: /tmp/frontend.zip
    dest: /usr/share/nginx/html
    remote_src: yes
```

**Module**: `unarchive`
**Purpose**: Extracts frontend content into NGINX html directory.

---

### 9️⃣ Remove default nginx config

```yaml
- name: remove default nginx conf
  ansible.builtin.file:
    path: /etc/nginx/nginx.conf
    state: absent
```

**Module**: `file`
**Purpose**: Removes the default nginx.conf to prepare for a custom config.

---

### 10️⃣ Copy custom nginx config

```yaml
- name: copy roboshop nginx conf
  ansible.builtin.copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
```

**Module**: `copy`
**Purpose**: Deploys a custom nginx.conf optimized for the eCommerce application.

---

### 11️⃣ Restart nginx

```yaml
- name: restart nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
```

**Module**: `service`
**Purpose**: Restarts nginx to apply the new configuration and serve the frontend.

---

## Why This Playbook Is Useful

* Automates **NGINX setup and configuration** — saves a lot of manual effort.
* Ensures correct **NGINX version** is installed and configured.
* Enables **fast deployment** of updated frontend code.
* Allows **custom nginx config** to be deployed in a repeatable way.
* Promotes **Infrastructure as Code** and makes frontend deployment consistent across environments (Dev, QA, Prod).

---

## Real-World Scenario

👉 In a microservices eCommerce app, frontend is a **single-page application** served via nginx.

👉 The backend components (catalogue, user, cart, payment) interact with it via API calls.

👉 DevOps can use this playbook to:

✅ Provision new frontend servers on cloud or on-prem
✅ Deploy latest frontend build easily
✅ Apply tested nginx config consistently
✅ Ensure service is always running after deployment

👉 This removes the risk of **manual nginx misconfiguration** or inconsistent deployments.
