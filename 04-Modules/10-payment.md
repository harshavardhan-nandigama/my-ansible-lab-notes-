
# Configure Payment Component

## What this file does

This playbook automates the **deployment and configuration of the `payment` component** of the eCommerce application.

It performs:

✅ System package installation
✅ Application directory setup
✅ User management
✅ Application code download and extraction
✅ Python dependency installation
✅ Systemd service setup and management

| Module      | Purpose                                       |
| ----------- | --------------------------------------------- |
| `dnf`       | Install Python and compiler dependencies      |
| `file`      | Create application directory                  |
| `user`      | Create system user                            |
| `get_url`   | Download application code                     |
| `unarchive` | Extract application zip                       |
| `pip`       | Install Python dependencies from requirements |
| `copy`      | Copy systemd service unit                     |
| `systemd`   | Reload systemd daemon                         |
| `service`   | Start and enable the service                  |

---

## Playbook Header Block

```yaml
- name: configure payment server
  become: yes
  hosts: payment
```

---

## Explanation of key tasks

### 1️⃣ Install system dependencies

```yaml
- name: install python3 packages 
  ansible.builtin.dnf:
    name: "{{ item }}"
    state: installed 
  loop: 
  - python3
  - gcc
  - python3-devel
```

**Module**: `dnf`
**Purpose**: Installs necessary system packages to build and run Python applications:

* `python3`: Python runtime
* `gcc`: C compiler (required by some Python packages)
* `python3-devel`: Python headers for compiling Python modules

---

### 2️⃣ Create application directory

```yaml
- name: create app directory
  ansible.builtin.file:
    path: /app
    state: directory
```

**Module**: `file`
**Purpose**: Creates `/app` directory to host the payment app code.

---

### 3️⃣ Create roboshop system user

```yaml
- name: create roboshop system user
  ansible.builtin.user:
    name: roboshop
    shell: /sbin/nologin
    system: true
    home: /app 
```

**Module**: `user`
**Purpose**: Creates `roboshop` system user with `/app` as home directory, no login shell.

---

### 4️⃣ Download payment code

```yaml
- name: donwload payment code
  ansible.builtin.get_url:
    url: https://roboshop-artifacts.s3.amazonaws.com/payment-v3.zip 
    dest: /tmp/payment.zip
```

**Module**: `get_url`
**Purpose**: Downloads the payment component ZIP file from S3.

---

### 5️⃣ Extract payment code

```yaml
- name: extract payment code
  ansible.builtin.unarchive:
    src: /tmp/payment.zip
    dest: /app
    remote_src: yes
```

**Module**: `unarchive`
**Purpose**: Extracts payment code into `/app`.

---

### 6️⃣ Install Python dependencies

```yaml
- name: install dependencies
  ansible.builtin.pip:
    requirements: requirements.txt
    executable: pip3.9
  args: 
    chdir: /app
```

**Module**: `pip`
**Purpose**: Installs required Python packages listed in `requirements.txt` using Python 3.9.
The app will not function without these.

---

### 7️⃣ Copy payment service

```yaml
- name: copy payment service
  ansible.builtin.copy:
    src: payment.service
    dest: /etc/systemd/system/payment.service
```

**Module**: `copy`
**Purpose**: Deploys the systemd unit file for `payment` service.

---

### 8️⃣ Reload systemd daemon

```yaml
- name: daemon reload
  ansible.builtin.systemd:
    daemon_reload: true
```

**Module**: `systemd`
**Purpose**: Reloads systemd to pick up the new payment service.

---

### 9️⃣ Enable and start payment service

```yaml
- name: enable and start payment
  ansible.builtin.service:
    name: payment
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts the `payment` service and ensures it starts automatically on boot.

---

## Why This Playbook Is Useful

* Automates **payment component deployment** across environments.
* Ensures consistent and error-free installation.
* Manages all service dependencies (Python, GCC, systemd service).
* Makes it easy to update and redeploy the payment component.
* Part of a larger **CI/CD pipeline** for eCommerce microservices.

---

## Real-World Scenario

👉 In an **eCommerce architecture**, the `payment` service handles:

* Payment processing
* Order validation
* Payment confirmation updates

👉 With this playbook, a DevOps engineer can:
✅ Deploy payment service on new nodes in minutes
✅ Update service version with one command (`ansible-playbook`)
✅ Eliminate manual configuration errors
✅ Easily roll out payment component during app upgrades

