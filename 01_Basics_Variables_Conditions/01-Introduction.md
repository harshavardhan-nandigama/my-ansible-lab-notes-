# Ansible

Welcome to my personal **Ansible learning journey**! This repository contains structured, hands-on labs over 7 days — covering everything from Ansible basics to automation with real-world examples.

## What is Ansible?

**Ansible** is an open-source IT automation tool used for:

- ✅ Configuration Management
- ✅ Application Deployment
- ✅ Orchestration
- ✅ Infrastructure as Code (IaC)

It helps DevOps engineers and system administrators automate complex tasks across multiple systems in a **simple, efficient, and repeatable way**.


## Why Use Ansible?

|      Feature      |                          Benefit                                        |
|-----------------  |------------------------------------------------------------------------ |
| ✅ Agentless      | No need to install any agent on client machines (uses SSH)             |
| ✅ Simple Syntax  | YAML-based playbooks are easy to read and write                        |
| ✅ Idempotent     | Safe to run repeatedly — the end state remains the same                |
| ✅ Scalable       | Manage thousands of servers from a single control node                 |
| ✅ Cloud-Ready    | Integrates with AWS, Azure, GCP, Docker, Kubernetes, and more          |


## How Ansible Works

Ansible works on a **control node** (your machine) that connects to **managed nodes** (target servers) using **SSH**. You define tasks in **YAML playbooks** that Ansible executes on remote hosts.

        
        - name: Install Apache Web Server
        hosts: webservers
        become: yes
        tasks:
            - name: Install httpd
            yum:
                name: httpd
                state: present


## Core Components of Ansible

|   Component   |                 Description                              |
| ------------- | -------------------------------------------------------- |
| **Inventory** | List of target hosts (e.g., IP addresses or hostnames)   |
| **Playbook**  | YAML file with automation instructions                   |
| **Task**      | Single unit of work (e.g., install package)              |
| **Module**    | Reusable Ansible feature to perform tasks                |
| **Handler**   | Task triggered by a notification (e.g., restart service) |
| **Role**      | Organized structure for reusable automation              |
| **Facts**     | System data collected from managed hosts                 |


## Real-World Use Cases

- Installing packages and services (e.g., Nginx, MySQL)

- Managing system users and permissions

- Deploying applications across environments

- Configuring firewalls and networking

- Provisioning servers on AWS, GCP, or Azure

- Managing Docker containers and Kubernetes clusters

## Learning Goals in This Repository 

-  Understand Ansible basics and YAML syntax

-  Create and manage inventories and playbooks

-  Use variables, loops, and conditionals

-  Organize code with roles and handlers

- Work with modules and facts

-  Automate real-world DevOps tasks


