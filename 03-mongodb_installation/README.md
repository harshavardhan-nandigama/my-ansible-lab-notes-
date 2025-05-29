# 03- MongoDB Installation using Ansible



This playbook demonstrates how to **automate MongoDB installation** on a Red Hat-based Linux system using Ansible. 

---

## Objective

- Install MongoDB
- Enable and start the service
- Allow external (remote) connections by updating `mongod.conf`


## Playbook Structure: `01-mongodb-playbook.yml`

| Step | Task Description |
|------|------------------|
| 1️⃣   | Copies the MongoDB repository file to the target |
| 2️⃣   | Installs `mongodb-org` using `dnf` |
| 3️⃣   | Enables and starts the `mongod` service |
| 4️⃣   | Edits `mongod.conf` to allow connections from any IP (`0.0.0.0`) |
| 5️⃣   | Restarts MongoDB to apply changes |

---

## 📁 Files Included

|    File Name              |          Purpose                            |
|------------------------   |---------------------------------------------|
| `01-mongodb-playbook.yml` | Main playbook with all automation steps     |
| `mongo.repo`              | Custom YUM repo to install MongoDB          |
| `README.md`               | This file with full explanation             |

---

## Note

- Ensure the host group `[mongodb]` is defined in your inventory file.
- This playbook assumes **RHEL/CentOS based systems**.
- You must have passwordless `sudo` access on the target machine.


##  Sample Inventory (`inventory.ini`)

```ini
[mongodb]
192.168.1.10
