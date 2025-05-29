# 03- Ansible Loops 

## What Are Loops in Ansible?

In Ansible, **loops** allow you to repeat a task multiple times with different values.

Think of it as:  
> “Write once, run many times.”

Instead of writing the same task again and again for different items (like users, packages, files), we use loops to simplify and automate.

### Why Loops Matter in Ansible

| Benefit                       |           Explanation                                                   |
| ------------------------------| ----------------------------------------------------------------------- |
|  **Automates repetition**     | Run a task multiple times without repeating code                        |
|  **Install many packages**    | Useful for batch installing packages like `httpd`, `nginx`, `php`, etc. |
|  **Create multiple users**    | Loop over user definitions to create accounts dynamically               |
|  **Clean multiple files**     | Remove or back up several files with one task                           |
|  **Write scalable playbooks** | Avoid code duplication and make your playbooks easy to maintain         |




### Basic Loop Syntax (Recommended)


        - name: Do something repeatedly
        module_name:
            parameter: "{{ item }}"
        loop:
            - value1
            - value2
            - value3




#🔥 Real-World Examples

## Example 1: Install Multiple Packages (Red Hat)

        - name: Install required packages
        yum:
            name: "{{ item }}"
            state: present
        loop:
            - httpd
            - php
            - mariadb-server

## Example 2: Create Multiple Users

- name: Create team users
  user:
    name: "{{ item }}"
    state: present
  loop:
    - mahi
    - bob
    - charlie

## Example 3: Copy Multiple Files

        - name: Copy config files to /etc/
        copy:
            src: "files/{{ item }}"
            dest: "/etc/{{ item }}"
        loop:
            - sshd_config
            - hosts
            - resolv.conf


# Advanced Loops

### Looping Over Dictionaries

    - name: Create users with UID
    user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        state: present
    loop:
        - { name: 'admin', uid: 1101 }
        - { name: 'developer', uid: 1102 }

### Loop with Index

        - name: Show loop index
        debug:
            msg: "Processing {{ item }} at index {{ ansible_loop.index }}"
        loop:
            - one
            - two
            - three
        loop_control:
            label: "{{ item }}"

        
###  with_items (Deprecated Style)

    - name: Install packages (old method)
    yum:
        name: "{{ item }}"
        state: present
    with_items:
        - git
        - vim


## When to Use Loops

| 🔧 Task            |  Module Example                     |
| ------------------ | ----------------------------------- |
| Install packages   | `yum` + `loop`                      |
| Create users       | `user` + `loop`                     |
| Copy files         | `copy` + `loop`                     |
| Configure services | `template` or `lineinfile` + `loop` |
| Restart services   | `service` + `loop`                  |




