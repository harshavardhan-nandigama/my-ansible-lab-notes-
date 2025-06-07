

# Setup RabbitMQ Component

## What this file does

This playbook automates the **setup and configuration of the `rabbitmq` component** used for messaging between microservices in the eCommerce system.

It uses Ansible modules to:

✅ Configure RabbitMQ repository
✅ Install RabbitMQ server
✅ Enable and start the RabbitMQ service
✅ Create a dedicated RabbitMQ user with necessary permissions

| Module                             | Purpose                                     |
| ---------------------------------- | ------------------------------------------- |
| `copy`                             | Copy RabbitMQ repo config                   |
| `dnf`                              | Install RabbitMQ server                     |
| `service`                          | Start and enable RabbitMQ service           |
| `community.rabbitmq.rabbitmq_user` | Create RabbitMQ user and assign permissions |

---

## Playbook Header Block

```yaml
- name: setup rabbitmq component
  become: yes
  hosts: rabbitmq
```

---

## Explanation of key tasks

### 1️⃣ Copy RabbitMQ repository file

```yaml
- name: copy rabbitmq repo
  ansible.builtin.copy:
    src: rabbitmq.repo
    dest: /etc/yum.repos.d/rabbit.repo
```

**Module**: `copy`
**Purpose**: Copies custom RabbitMQ repo file so that the correct version of RabbitMQ server can be installed.

---

### 2️⃣ Install RabbitMQ server

```yaml
- name: install rabbitmq.server
  ansible.builtin.dnf:
    name: rabbitmq-server
    state: installed
```

**Module**: `dnf`
**Purpose**: Installs **RabbitMQ server** package on the node.

---

### 3️⃣ Enable and start RabbitMQ service

```yaml
- name: enalbe and start rabbitmq
  ansible.builtin.service:
    name: rabbitmq-server
    state: started
    enabled: yes
```

**Module**: `service`
**Purpose**: Starts RabbitMQ server and ensures it is enabled on system boot.

---

### 4️⃣ Create RabbitMQ user

```yaml
- name: create rabbitmq user
  community.rabbitmq.rabbitmq_user:
    user: roboshop
    password: roboshop123
    permissions:
    - vhost: /
      configure_priv: .*
      read_priv: .*
      write_priv: .*
    state: present
```

**Module**: `community.rabbitmq.rabbitmq_user`
**Purpose**: Creates a RabbitMQ user named `roboshop` with full permissions on `/` vhost.

---

## Why This Playbook Is Useful

* Provides a **repeatable, automated setup** of RabbitMQ in production or test environments.
* Ensures RabbitMQ server is correctly configured and started.
* Handles **user creation and permission setup** without manual intervention.
* Makes RabbitMQ deployment **CI/CD-friendly**.

---

## Real-World Scenario

👉 In an **eCommerce microservices architecture**, RabbitMQ acts as a **message broker**.

* Used for **asynchronous communication** between services:

  * Order → Shipping
  * Payment → Notification
  * Inventory → Catalogue updates

👉 Using this playbook, a DevOps team can:

✅ Deploy RabbitMQ consistently across environments
✅ Automate user and permission setup
✅ Avoid manual RabbitMQ configuration errors
✅ Support **HA RabbitMQ cluster provisioning** if extended with more tasks

