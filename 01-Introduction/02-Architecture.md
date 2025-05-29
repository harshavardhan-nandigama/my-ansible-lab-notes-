# Ansible Architecture

Ansible is a si
mple, agentless automation tool used for configuration management, application deployment, and orchestration.

## Key Components

### 1. Control Node

- The machine where Ansible is installed.

- Executes Ansible commands and playbooks.

- Can be your local system, a VM, or a cloud instance (like AWS EC2).

### 2. Managed Nodes (Target Hosts)

- The systems Ansible manages.

- No need to install Ansible here.

- Communicated via SSH (Linux) or WinRM (Windows).

### 3. Inventory

- A file that lists all managed nodes (IP addresses or hostnames).

- Can be in INI, YAML, or dynamic (from cloud providers) format.

example

        [web]
        192.168.1.10
        192.168.1.11

        [db]
        192.168.1.12

### 4. Modules

- Units of work that Ansible executes on the managed nodes.

- Examples: ping, copy, yum, service, user, etc.

- Can be built-in, custom, or community modules.

### 5. Playbooks

- YAML files that define a series of tasks to be executed on managed nodes.

- Structured using:

- Plays (which hosts to target)

- Tasks (what to do)

- Modules (how to do it)


**example:**

    - name: Install Apache
    hosts: web
    become: true
    tasks:
        - name: Install httpd
        yum:
            name: httpd
            state: present


### 6. Tasks

- Single actions executed using a module.

- Defined inside a playbook under tasks.

Folder structure:

                roles/
        └── webserver/               # Name of the role
            ├── tasks/              # Contains the main list of tasks to run
            ├── handlers/           # Contains handlers (e.g., restart services)
            ├── templates/          # Jinja2 templates (.j2) for config files
            ├── files/              # Static files (e.g., index.html)
            ├── vars/               # Variables with higher priority
            ├── defaults/           # Default variables (lowest priority)
            └── meta/               # Metadata like dependencies, author info



### How Ansible Works (Flow)

- User runs a playbook from the control node.

- Ansible connects to managed nodes via SSH/WinRM.

- Executes modules as defined in playbooks.

- Collects and displays results to the user.


### Ansible Is:

✅ Agentless: No need to install software on target nodes.

✅ Push-Based: Executes commands from the control node.

✅ Declarative: You define what you want, Ansible figures out how to do it.



 ### Diagram
                                        ┌──────────────┐
                                        │ Control Node │
                                        └────┬─────────┘
                                            │
                            ┌─────────────┼──────────────────┐
                            │             │                  │
                        ┌──────▼─────┐ ┌─────▼──────┐     ┌─────▼─────┐
                        │ Managed 1  │ │ Managed 2  │ ... │ Managed N │
                        └────────────┘ └────────────┘     └───────────┘

--








