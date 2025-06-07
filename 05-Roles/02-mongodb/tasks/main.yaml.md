

## MongoDB Role Overview

This Ansible role automates the installation and configuration of the MongoDB server on your target hosts defined in the `mongodb` group. It follows best practices for installing MongoDB on a RedHat-based system using the official MongoDB repository.


### Tasks Breakdown

1. **Copy MongoDB Repository File**

```yaml
- name: copy mongodb repo
  ansible.builtin.copy:
    src: mongo.repo
    dest: /etc/yum.repos.d/mongo.repo
```

* Copies the `mongo.repo` file from your Ansible control node to the remote server.
* This file configures yum to use the official MongoDB repo so you can install the latest MongoDB packages easily.

2. **Install MongoDB Server**

```yaml
- name: Install mongodb server
  ansible.builtin.dnf:
    name: mongodb-org
    state: present
```

* Uses `dnf` to install the `mongodb-org` package, which includes the MongoDB server and tools.

3. **Start and Enable MongoDB Service**

```yaml
- name: start and enable mongod
  ansible.builtin.service:
    name: mongod
    state: started
    enabled: yes
```

* Starts the MongoDB service (`mongod`) and ensures it will start on boot.

4. **Allow Remote Connections**

```yaml
- name: allow remote connections
  ansible.builtin.replace:
    path: /etc/mongod.conf
    regexp: '127.0.0.1'
    replace: '0.0.0.0'
```

* Edits the MongoDB configuration file to replace the default bind IP address `127.0.0.1` (localhost only) with `0.0.0.0` to allow connections from any IP address.
* This is important for your distributed setup where other components or users connect remotely.

5. **Restart MongoDB Service**

```yaml
- name: restart mongodb
  ansible.builtin.service:
    name: mongod
    state: restarted
```

* Restarts the MongoDB service to apply the configuration changes.


