# 02- Ansible Data Types 

## What Are Data Types in Ansible?

In Ansible, variables can hold **different kinds of values** — like numbers, text, or lists.  
These are known as **data types**.

Understanding them helps you write better playbooks, avoid errors, and control task execution smartly.


## Why Should You Care?

- Data types help Ansible know **how** to treat the value  
- They’re **used in conditions, loops, templates, roles, etc.**  
- Used correctly, they make your playbooks readable, flexible, and reusable


## 🗂️ Common Data Types in Ansible

| Data Type   |       Description                    |         Example                  |
|-------------|--------------------------------------|----------------------------------|
| **String**  | A line of text                       | `"hello"`, `'RHEL 9'`            |
| **Integer** | A whole number                       | `80`, `0`, `-1`                  |
| **Boolean** | True or false value                  | `true`, `false`                  |
| **List**    | Ordered items (like an array)        | `[ "nginx", "firewalld" ]`       |
| **Dict**    | Key-value pairs (like JSON objects)  | `{ user: "admin", uid: 1010 }`   |
| **Null**    | An empty or undefined value          | `null`, `~`                      |

---

## 1. String

 A single piece of text inside quotes.


        os_name: "Red Hat Enterprise Linux"

**2. Integer**   

A numeric value without quotes.

        port: 8080

 **3. Boolean**

 A binary value: true or false.

        firewall_enabled: true
        selinux_mode: false

Useful in conditional execution (when: statements)

**4. List (Array)**

        packages:
        - httpd
        - php
        - mysql

Useful for installing multiple packages using loops.


**5. Dictionary (Key-Value)**

A set of key: value pairs, like a mini-database.

        user_info:
        name: admin
        uid: 1010
        shell: /bin/bash




**🚫 6. Null**

Represents an empty or undefined value.

        optional_value: null
        another_way: ~


Use when you want to leave something blank intentionally.


### Practical Example – Use All Types Together

        - name: Ansible Data Type Demo
        hosts: all
        become: true

        vars:
            os_name: "Red Hat Enterprise Linux"   # string
            port: 80                              # integer
            firewall_enabled: true                # boolean
            packages:                             # list
            - httpd
            - firewalld
            user_info:                            # dictionary
            name: apache
            uid: 48
            shell: /sbin/nologin
            log_dir: null                         # null

        tasks:
            - name: Show OS name
            debug:
                msg: "OS is {{ os_name }}"

            - name: Show port number
            debug:
                msg: "Listening on port {{ port }}"

            - name: Show firewall status
            debug:
                msg: "Firewall enabled? {{ firewall_enabled }}"

            - name: Print packages list
            debug:
                var: packages

            - name: Print user info dictionary
            debug:
                var: user_info

            - name: Check if log_dir is null
            debug:
                msg: "log_dir is not set"
            when: log_dir is not defined or log_dir is none


## Debugging Tips

You can use the debug module to check types:

    - name: Debug a variable
    debug:
        var: your_variable

**Or check everything at once:**

        ansible -i inventory all -m setup


### Summary

| Type       | Symbol / Format  |       Use Case               |
| ---------- | ---------------- | ---------------------------- |
| String     | `"text"`         | Labels, paths, config values |
| Integer    | `123`            | Ports, counts, retries       |
| Boolean    | `true` / `false` | Toggles, conditions          |
| List       | `[...]`          | Loops, batch tasks           |
| Dictionary | `{ key: value }` | Structured settings          |
| Null       | `null` or `~`    | Optional or empty values     |





