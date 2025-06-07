

## MySQL Role Tasks Explanation

This role automates the installation, startup, and basic secure setup of the MySQL server on your target hosts.

### Tasks Breakdown

1. **Install MySQL Server**

```yaml
- name: Install MySQL server
  ansible.builtin.dnf:
    name: mysql-server
    state: installed
```

* Installs the MySQL server package using `dnf`.
* Ensures the MySQL server is present on the system.

2. **Start and Enable MySQL Service**

```yaml
- name: start mysql server
  ansible.builtin.systemd_service:
    name: mysqld
    state: started
    enabled: yes
```

* Starts the `mysqld` service immediately.
* Enables the service to start automatically on system boot.

3. **Secure MySQL Installation and Set Root Password**

```yaml
- name: setup root password
  ansible.builtin.command: mysql_secure_installation --set-root-pass RoboShop@1
```

* Runs the MySQL secure installation script to set the root password.
* This helps to secure MySQL by setting a root password and applying basic security configurations.

---

### Notes:

* Using `mysql_secure_installation --set-root-pass` in a non-interactive way is convenient but be sure your MySQL version supports this option; otherwise, you might need to use other methods (like SQL commands) to set root password.
* After this step, the root password will be `RoboShop@1` for connecting to the MySQL server.


