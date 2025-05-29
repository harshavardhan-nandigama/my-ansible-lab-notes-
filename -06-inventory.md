# 📋 Ansible Inventory Basics

##  What is an Inventory?

An **inventory** in Ansible is a **list of hosts (servers)** that you want to manage.  
You tell Ansible **which servers to target** using an inventory file.

>  Think of it as your address book of servers.

Ansible reads this file to know **where to run your automation**.

---

## Types of Inventory

### 1️⃣ Static Inventory

This is a **simple text file** (usually `.ini` format) with a list of IPs or hostnames grouped into categories.

###  Example: `inventory.ini`

        [webservers]
        192.168.1.10
        192.168.1.11

        [dbservers]
        db1.example.com
        db2.example.com

 **Run a playbook using this static inventory:**

    ansible-playbook -i inventory.ini playbook.yml


### 2️⃣ Dynamic Inventory
Used in cloud environments like AWS, Azure, GCP.
The inventory is generated dynamically using scripts or plugins.

For example, in AWS, you can pull EC2 instance IPs dynamically using an inventory plugin or script.

**Example:**

        ansible-inventory -i aws_ec2.yml --graph

Requires boto3, and AWS credentials setup.

### Advanced Static Inventory Example (with variables)

        [web]
        web1 ansible_host=192.168.0.10 ansible_user=ubuntu
        web2 ansible_host=192.168.0.11 ansible_user=ubuntu

        [db]
        db1 ansible_host=192.168.0.12 ansible_user=admin ansible_port=2222


- ansible_host: IP/hostname to connect to

- ansible_user: Remote SSH user

- ansible_port: Optional SSH port

**📍 Where to Place Inventory File?**

- In the same directory as your playbook

- Or specify its path using -i flag

**Summary**


- Inventory tells Ansible which machines to manage.

- Static inventory: Best for local or fixed IP environments.

- Dynamic inventory: Best for cloud-based dynamic systems.

- Inventory can also hold host variables like SSH user or port.






