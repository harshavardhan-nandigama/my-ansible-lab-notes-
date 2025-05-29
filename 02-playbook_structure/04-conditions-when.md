# 04- Conditions(when)

**What are Conditions?**

In Ansible, conditions are used to control the execution of tasks based on certain values, facts, or expressions. The when keyword is used to apply a condition.

It helps make playbooks smarter by running tasks only when specific conditions are met.

 

 **Example 1: Simple Condition**

        - name: Install Apache only in production
        ansible.builtin.yum:
            name: httpd
            state: present
        when: app_env == "production"

**Example 2: Using Ansible Facts**

        - name: Install NGINX if OS is Debian
        ansible.builtin.apt:
            name: nginx
            state: present
        when: ansible_os_family == "Debian"

**Example 3: Multiple Conditions**

        - name: Restart service if enabled and on prod
        ansible.builtin.service:
            name: myapp
            state: restarted
        when: is_enabled and app_env == "production"

**Common Use Cases**

| Use Case                  |     Example Expression          |
| ------------------------- | ------------------------------- |
| Based on OS               | `ansible_os_family == "RedHat"` |
| Based on variable value   | `app_env == "production"`       |
| Based on fact             | `ansible_memtotal_mb > 1024`    |
| Multiple conditions (AND) | `a == 1 and b == 2`             |
| Multiple conditions (OR)  | `x == "yes" or y == "true"`     |
| Boolean variable          | `is_enabled`                    |


