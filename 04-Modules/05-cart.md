

#  Configure Cart Component

## What this file does

This playbook automates the **deployment and configuration of the `cart` component** of an eCommerce application.
It handles:

✅ Managing Node.js version modules
✅ Creating system user and app directory
✅ Downloading and deploying the cart application code
✅ Installing Node.js dependencies
✅ Configuring and starting the cart systemd service

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
| `systemd_service`       | Reload systemd daemon         |
| `service`               | Start and enable cart service |

---

## Playbook Header Block

```yaml
- name: configuring cart component
  hosts: cart
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
- name: Install nodejs
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

### 6️⃣ Download cart code

```yaml
- name: download cart code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/cart-v3.zip
    dest: /tmp/cart.zip
```

**Module**: `get_url`
**Purpose**: Downloads the cart component zip archive from the S3 bucket.

---

### 7️⃣ Extract cart code

```yaml
- name: extract cart code
  ansible.builtin.unarchive:
    src: /tmp/cart.zip
    dest: /app
    remote_src: yes
```

**Module**: `unarchive`
**Purpose**: Extracts the downloaded zip into `/app`.

---

### 8️⃣ Install Node.js dependencies

```yaml
- name: install dependenceies
  community.general.npm:
    path: /app
```

**Module**: `community.general.npm`
**Purpose**: Installs Node.js dependencies using npm in `/app`.

---

### 9️⃣ Copy cart systemd service file

```yaml
- name: copy cart service to system directory
  ansible.builtin.copy:
    src: cart.service
    dest: /etc/systemd/system/cart.service
```

**Module**: `copy`
**Purpose**: Copies the cart service systemd unit file to the system directory.

---

### 🔟 Reload systemd daemon

```yaml
- name: systemctl daemon reload
  ansible.builtin.systemd_service:
    daemon_reload: true
```

**Module**: `systemd_service`
**Purpose**: Reloads systemd to recognize the new or changed service file.

---

### 1️⃣1️⃣ Start and enable cart service

```yaml
- name: start and enable cart
  ansible.builtin.service:
    name: cart
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts the cart service and enables it to start automatically on boot.

---

## Why This Playbook Is Useful

* Automates consistent deployment of the cart microservice on multiple servers.
* Manages Node.js versioning and dependencies seamlessly.
* Creates necessary system user and directory setup for secure app hosting.
* Automates service registration and startup to ensure high availability.
* Reduces manual steps and errors during cart component deployment.

---

## Real-World Scenario

👉 In a large-scale eCommerce platform:

* The cart service handles shopping cart functionality (adding/removing items, checkout process).
* Needs Node.js runtime and correct dependencies.
* This playbook enables DevOps teams to quickly deploy or redeploy the cart service during updates or scaling.
* Ensures zero downtime by managing the service lifecycle automatically.
* Provides repeatable, scalable infrastructure automation for microservices architecture.
