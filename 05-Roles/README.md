# Roles Concept with Roboshop Project

## Objective

On Day 5 of my Ansible learning journey, I implemented **Ansible Roles** to automate the deployment of a complete microservices-based ecommerce project - **Roboshop**.

This project demonstrates how to:

✅ Structure Ansible code using **roles**
✅ Reuse common tasks with `common` role
✅ Automate deployment of **10 components**:

* MongoDB
* MySQL
* RabbitMQ
* Redis
* Catalogue
* User
* Cart
* Shipping
* Payment
* Frontend

---

## Architecture

```plaintext
                              +-------------------+
                              |     Frontend      |
                              |    (NGINX)        |
                              +-------------------+
                                         |
    -------------------------------------------------------------------------
    |                  |                   |                   |
    v                  v                   v                   v
+------------+   +------------+   +------------+   +------------+
| Catalogue  |   | User       |   | Cart       |   | Payment    |
| NodeJS     |   | NodeJS     |   | NodeJS     |   | Python     |
+------------+   +------------+   +------------+   +------------+
      |                |               |                |
      v                v               v                v
  MongoDB         MongoDB          Redis + MySQL     RabbitMQ + MySQL

+------------+   +------------+   +------------+   +------------+
| Shipping   |   | Redis      |   | RabbitMQ   |   | MySQL      |
| Java/Maven |   |            |   |            |   |            |
+------------+   +------------+   +------------+   +------------+
```

---

## Project Structure

```plaintext
roles/
├── common/           --> common reusable tasks (app setup, nodejs, python, maven, systemd)
├── mongodb/
├── mysql/
├── rabbitmq/
├── redis/
├── catalogue/
├── user/
├── cart/
├── shipping/
├── payment/
├── frontend/
inventory.ini         --> Inventory file
main.yaml             --> Main playbook using roles
README.md             --> Project README
```

---

##  How to Run

### Prerequisites:

* Ansible installed
* EC2 instances configured (as per `inventory.ini`)

### Run a specific component:

```bash
ansible-playbook -i inventory.ini -e "component=cart" main.yaml
ansible-playbook -i inventory.ini -e "component=shipping" main.yaml
ansible-playbook -i inventory.ini -e "component=frontend" main.yaml
```

### Example run:

```bash
ansible-playbook -i inventory.ini -e "component=catalogue" main.yaml
```

---

## Common Role Usage

The `common` role contains reusable tasks for:

✅ App directory setup
✅ Roboshop system user creation
✅ Download & extract code
✅ NodeJS installation & dependency install
✅ Python dependency install
✅ Maven build
✅ Systemd service management

Each component playbook includes:

```yaml
- name: app setup
  include_role:
    name: common
    tasks_from: app-setup
```

---

## Components Automated

✅ MongoDB
✅ MySQL
✅ RabbitMQ
✅ Redis
✅ Catalogue (NodeJS)
✅ User (NodeJS)
✅ Cart (NodeJS)
✅ Shipping (Java/Maven)
✅ Payment (Python)
✅ Frontend (NGINX + HTML/JS)

---



## Summary

This was my **Ansible Project** focused on:

✅ Understanding and using **Roles**
✅ Structuring Ansible code
✅ Reusing code across multiple components
✅ Managing a complete **microservices architecture**
✅ Deploying using **Ansible Automation**


