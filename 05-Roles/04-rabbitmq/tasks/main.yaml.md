
## RabbitMQ Role Tasks Explanation

This role automates the installation, configuration, and user setup for **RabbitMQ**, a popular message broker used in the RoboShop project.

### Tasks Breakdown

---

### 1️⃣ Copy RabbitMQ Repository

```yaml
- name: copy rabbitmq repo
  ansible.builtin.copy:
    src: rabbitmq.repo
    dest: /etc/yum.repos.d/rabbit.repo
```

* Copies a pre-configured RabbitMQ YUM repository file to the system.
* This allows the system to download and install the latest RabbitMQ server from the official or custom repository.

---

### 2️⃣ Install RabbitMQ Server

```yaml
- name: install rabbitmq.server
  ansible.builtin.dnf:
    name: rabbitmq-server
    state: installed
```

* Installs the RabbitMQ server package using `dnf`.

---

### 3️⃣ Enable and Start RabbitMQ Service

```yaml
- name: enalbe and start rabbitmq
  ansible.builtin.service:
    name: rabbitmq-server
    state: started
    enabled: yes
```

* Starts the RabbitMQ server immediately.
* Enables the service to automatically start on system boot.

---

### 4️⃣ Create RabbitMQ Application User

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

* Creates a **RabbitMQ user** named `roboshop` with password `roboshop123`.
* Grants the user full permissions (configure, read, write) on the default virtual host `/`.

---

