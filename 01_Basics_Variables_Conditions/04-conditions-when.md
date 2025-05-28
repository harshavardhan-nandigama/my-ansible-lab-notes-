#                           Ansible Conditions - The `when` Statement 



##  What is a Condition?

In Ansible, a **condition** is like saying:

> "Do this only if something is true."

We use `when` in a task to apply this logic.



##  Real-Life Analogy

Imagine you're giving ice cream 🍦 to students, but **only to those who scored more than 90**.

You’ll say:
> _"Give ice cream when marks > 90."_

Ansible does the same using:


        when: marks > 90


## Why Use Conditions in Ansible?

_ Run tasks only when needed

- Avoid unnecessary steps

- Make your playbooks smarter and efficient

## Example 1 – Run Task Only on Red Hat Systems

        - name: Install httpd only on Red Hat/CentOS
        hosts: web
        become: yes

        tasks:
            - name: Install httpd (Red Hat)
            yum:
                name: httpd
                state: present
            when: ansible_facts['os_family'] == "RedHat"


## Example 2 – Conditional Task with Custom Variable

        - name: Conditional Package Install
        hosts: web
        become: yes

        vars:
            install_web: true

        tasks:
            - name: Install httpd only if install_web is true
            yum:
                name: httpd
                state: present
            when: install_web

    
    
###  Use Cases for when

|    Use Case        |                 Condition                                  |
| ------------------ | ---------------------------------------------------------- |
| Run only on RHEL   | `when: ansible_facts['os_family'] == "RedHat"`             |
| Check a version    | `when: ansible_facts['distribution_major_version'] == "8"` |
| Use variable value | `when: env == "prod"`                                      |


### Summary

- when is used for conditional task execution

- Works with facts, variables, or both

- Makes your playbooks intelligent, OS-aware, and error-free

