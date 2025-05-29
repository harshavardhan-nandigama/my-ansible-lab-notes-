#                                               Ansible Variables 


##  What is a Variable?

A **variable** is like a **placeholder** or a **container** used to store information — such as names, paths, IP addresses, or any value that may change.

Instead of writing the same value multiple times, you define it once and use the variable name everywhere.


## Real-Life Analogy

Imagine you’re printing certificates for a class. You don’t want to write each student’s name manually.  
You use a variable like `{{ student_name }}` and just fill in the value for each student. Ansible works similarly.

## Why Use Variables in Ansible?

- Makes playbooks **easier to read**
- Helps avoid **repetition**
- Easier to **modify** or **reuse** code
- Useful in **loops**, **conditions**, and **templates**


# How to Define and Use Variables

### 1. Define inside a Playbook


        vars:
        package_name: nginx


### Use inside a Task

        - name: Install a package
        apt:
            name: "{{ package_name }}"
            state: present


### xample Playbook Using a Variable

        - name: Day 1 - Install Package using Variable
        hosts: web
        become: yes

        vars:
            package_name: nginx

        tasks:
            - name: Install package with variable
            apt:
                name: "{{ package_name }}"
                state: present


This playbook will install nginx using the variable package_name.

### Where Can You Define Variables?

|   Method      |          Description                      |
| ------------- | ----------------------------------------- |
| `vars:` block | Inside playbook                           |
| `vars_files:` | Import from external YAML file            |
| `group_vars/` | Variables for a group of hosts            |
| `host_vars/`  | Variables for specific hosts              |
| `-e` option   | Pass from command line (highest priority) |

### Summary

- Variables make your playbooks dynamic and reusable

- Defined using key: value format

- Accessed using double curly braces like {{ variable_name }}

- Can be defined in multiple place

