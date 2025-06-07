

#  Configure MongoDB Server

## What this file does

This playbook automates the **installation and configuration of a MongoDB server** for an eCommerce application.

It performs:

✅ MongoDB repo setup
✅ MongoDB server installation
✅ Service enable/start
✅ Configuring MongoDB for remote connections
✅ Restarting MongoDB after config changes

| Module    | Purpose                             |
| --------- | ----------------------------------- |
| `copy`    | Copy MongoDB repo file              |
| `dnf`     | Install MongoDB server              |
| `service` | Start and enable mongod service     |
| `replace` | Update bind IP to allow remote conn |
| `service` | Restart mongod service after config |

---

## Playbook Header Block

```yaml
- name: configuring mongodb server
  become: yes
  hosts: mongodb
```

---

## Explanation of key tasks

### 1️⃣ Copy MongoDB repo file

```yaml
- name: copy mongodb repo
  ansible.builtin.copy:
    src: mongo.repo
    dest: /etc/yum.repos.d/mongo.repo
```

**Module**: `copy`
**Purpose**: Adds the MongoDB YUM repository so that the correct MongoDB server version can be installed.

---

### 2️⃣ Install MongoDB server

```yaml
- name: Install mongodb server
  ansible.builtin.dnf:
    name: mongodb-org
    state: present
```

**Module**: `dnf`
**Purpose**: Installs MongoDB server (all required packages).

---

### 3️⃣ Start and enable mongod service

```yaml
- name: start and enable mongod
  ansible.builtin.service:
    name: mongod
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Ensures that MongoDB service is running and enabled on boot.

---

### 4️⃣ Allow remote connections

```yaml
- name: allow remote connections
  ansible.builtin.replace:
    path: /etc/mongod.conf
    regexp: '127.0.0.1'
    replace: '0.0.0.0'
```

**Module**: `replace`
**Purpose**: Updates MongoDB bind IP so it can accept remote connections from other application components (important for microservices architecture).

---

### 5️⃣ Restart MongoDB service

```yaml
- name: restart mongodb
  ansible.builtin.service:
    name: mongod
    state: restarted
```

**Module**: `service`
**Purpose**: Restarts MongoDB to apply the new configuration (bind IP changes).

---

## Why This Playbook Is Useful

* Automates **end-to-end setup** of MongoDB, saving time in server provisioning.
* Ensures that the service is **ready to accept remote connections**, which is critical in a distributed application.
* Guarantees **idempotency** — you can run it multiple times safely.
* Simplifies production deployments and allows **infrastructure as code** practices.

---

## Real-World Scenario

👉 In a real microservices-based eCommerce application:

* MongoDB acts as the **primary NoSQL database** to store product catalogue, orders, user data, etc.
* Needs to be accessible to multiple app components (catalogue, user, cart services).
* Often deployed on its own instance with remote connection enabled.
* This playbook allows **DevOps teams** to quickly provision or update MongoDB nodes in any environment (dev, staging, prod).
* Also useful for scaling horizontally — new nodes can be configured identically via this playbook.

