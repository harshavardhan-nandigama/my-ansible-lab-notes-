
# Roboshop eCommerce — Ansible Playbooks

This repo contains **Ansible playbooks** to deploy the `Roboshop` eCommerce microservices app on Linux servers.

✅ Fully automated
✅ Component-wise deployment
✅ Easy to re-run and manage

---

## Components Covered

| Component | Stack        |
| --------- | ------------ |
| frontend  | Nginx + HTML |
| mongodb   | MongoDB      |
| catalogue | Node.js      |
| redis     | Redis        |
| user      | Node.js      |
| cart      | Node.js      |
| mysql     | MySQL        |
| shipping  | Java + Maven |
| rabbitmq  | RabbitMQ     |
| payment   | Python       |

---

## Usage

1️⃣ Clone the repo:

```bash
git clone https://github.com/YOUR_USERNAME/roboshop-ansible.git
cd roboshop-ansible
```

2️⃣ Update `inventory.ini` with your instance IPs.

3️⃣ Run desired playbooks:

```bash
ansible-playbook -i inventory.ini frontend.yaml
ansible-playbook -i inventory.ini mongodb.yaml
ansible-playbook -i inventory.ini catalogue.yaml
# and so on...
```

---

## Why This Is Useful

✅ No manual deployment — **fully automated**
✅ Each component is independently deployable
✅ **CI/CD-friendly**
✅ Easy to update components by re-running playbooks

---

## Real-World Scenario

In real production environments, DevOps teams often deploy apps in **microservices** style:

* You may need to **deploy/update single services** frequently.
* Using these playbooks ensures that deployments are **consistent** and **repeatable**.
* Can be easily integrated into **pipelines** (Jenkins, GitHub Actions).

