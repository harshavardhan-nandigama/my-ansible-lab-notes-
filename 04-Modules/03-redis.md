

# Configure Redis Component

## What this file does

This playbook automates the **deployment and configuration of the `redis` component** of an eCommerce application.
It uses a combination of Ansible modules to:

✅ Manage Redis version
✅ Configure Redis for remote access
✅ Tune Redis settings (disable protected mode)
✅ Enable and start the Redis service

| Module       | Purpose                                     |
| ------------ | ------------------------------------------- |
| `command`    | Disable/Enable Redis module streams         |
| `dnf`        | Install Redis package                       |
| `replace`    | Replace bind address in redis.conf          |
| `lineinfile` | Change protected-mode setting in redis.conf |
| `service`    | Start and enable Redis service              |

---

## Playbook Header Block

```yaml
- name: install redis component
  hosts: redis
  become: yes
```

### Explanation:

* `name`: Name of the play → installing redis component
* `hosts`: Target inventory group → redis
* `become: yes`: Run tasks with root privileges (required for package installs and config changes)

---

## Explanation of key tasks

### 1️⃣ Disable default Redis module

```yaml
- name: disable redis default module
  ansible.builtin.command: dnf module disable redis -y
```

**Module**: `command`
**Purpose**: Disables the default Redis module to avoid version conflicts.

---

### 2️⃣ Enable Redis 7 module

```yaml
- name: enable redis:7
  ansible.builtin.command: dnf module enable redis:7 -y
```

**Module**: `command`
**Purpose**: Enables Redis version 7 stream to install the required version.

---

### 3️⃣ Install Redis

```yaml
- name: install redis
  ansible.builtin.dnf:
    name: redis
    state: present
```

**Module**: `dnf`
**Purpose**: Installs Redis 7 package on the server.

---

### 4️⃣ Allow remote connections

```yaml
- name: allow remote connections
  ansible.builtin.replace:
    path: /etc/redis/redis.conf
    regexp: '127.0.0.1'
    replace: '0.0.0.0'
```

**Module**: `replace`
**Purpose**: Modifies Redis bind address to `0.0.0.0` so Redis accepts connections from external systems (remote access).

---

### 5️⃣ Disable protected mode

```yaml
- name: disable protected mode
  ansible.builtin.lineinfile:
    path: /etc/redis/redis.conf
    regexp: 'protected-mode'
    line: protected-mode no
```

**Module**: `lineinfile`
**Purpose**: Disables Redis protected mode, required when allowing remote access.

---

### 6️⃣ Start and enable Redis service

```yaml
- name: enable and start redis
  ansible.builtin.service:
    name: redis
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts the Redis service and ensures it starts on boot.

---

## Why This Playbook Is Useful

* Automates the full deployment of the Redis server.
* Demonstrates practical configuration tuning using **replace** and **lineinfile** modules.
* Ensures **Redis is correctly installed**, **configured for remote access**, and **running as a service**.
* Saves time and prevents manual errors in Redis deployment.

---

## Real-World Scenario

👉 In a real-world eCommerce architecture:

* Redis is often used as a **fast in-memory key-value store**.
* It may be used for:

✅ Caching
✅ Session management
✅ Storing frequently accessed data (improves performance)

👉 This playbook ensures:

✅ Correct version of Redis is installed
✅ Remote access is allowed (for use by app components)
✅ Protected mode is disabled for external use
✅ Redis is running and auto-start enabled on reboot


