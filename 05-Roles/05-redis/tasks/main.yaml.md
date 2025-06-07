

## Redis Role Tasks Explanation

This role automates the installation and configuration of **Redis** for the RoboShop project.

---

### Tasks Breakdown

---

### 1️⃣ Disable Default Redis Module

```yaml
- name: disable redis default module
  ansible.builtin.command: dnf module disable redis -y
```

* Disables the default Redis module provided by the OS.
* Useful when you want to install a specific version (here Redis 7).

---

### 2️⃣ Enable Redis 7 Module

```yaml
- name: enable redis:7
  ansible.builtin.command: dnf module enable redis:7 -y
```

* Enables the Redis 7 stream so that the correct version can be installed.

---

### 3️⃣ Install Redis

```yaml
- name: install redis
  ansible.builtin.dnf:
    name: redis
    state: present
```

* Installs the Redis server package.

---

### 4️⃣ Allow Remote Connections

```yaml
- name: allow remote connections
  ansible.builtin.replace:
    path: /etc/redis/redis.conf
    regexp: '127.0.0.1'
    replace: '0.0.0.0'
```

* Updates Redis config to allow connections from any IP address, not just localhost.
* Required for external services to communicate with Redis.

---

### 5️⃣ Disable Protected Mode

```yaml
- name: disable protected mode
  ansible.builtin.lineinfile:
    path: /etc/redis/redis.conf
    regexp: 'protected-mode'
    line: protected-mode no
```

* Turns off Redis protected mode.
* Necessary when Redis is exposed to external connections.
* ⚠️ **Use with caution** in production!

---

### 6️⃣ Enable and Start Redis Service

```yaml
- name: enable and start redis
  ansible.builtin.service:
    name: redis
    state: started
    enabled: yes
```

* Starts the Redis service.
* Enables Redis to auto-start on system reboot.

---

### Summary:

✅ Install Redis 7
✅ Allow remote connections
✅ Disable protected mode
✅ Start and enable Redis service

