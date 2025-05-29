# Ansible "Functions" (Filters & Lookups)


## 📘 What Are Functions in Ansible?

In Ansible, you don't write functions like in Python or Java.  
Instead, you use **filters** and **lookups**, which **act like built-in functions** to:

- 🔁 Transform data (e.g., change text to uppercase)
- 📤 Get data from files or the environment
- 🔍 Process lists, strings, numbers, and more

These tools are part of **Jinja2 templating**, which is built into Ansible.


#  1. Filters (Like Built-in Functions)

Filters are used with variables to transform or process them using the pipe symbol `|`.


**What are Filters?**

Filters are like functions that you use to modify or transform data (text, numbers, lists, etc.) inside your playbook.

**Example:**
If you have a name:

    name: "rahul"

You can use the upper filter to make it uppercase:


        {{ name | upper }}

 **Output:**


        RAHUL

### Example 1: Convert String to Uppercase


    - name: Convert string to uppercase
    hosts: localhost
    gather_facts: no
    tasks:
        - name: Uppercase example
        debug:
            msg: "{{ 'ansible' | upper }}"

**Output**

    ok: [localhost] => {
        "msg": "ANSIBLE"
    }



## Example 2: Add Two Numbers

    - name: Add two string numbers
    hosts: localhost
    gather_facts: no
    vars:
        x: "5"
        y: "10"
    tasks:
        - name: Add numbers
        debug:
            msg: "{{ x | int + y | int }}"

**Output**

        ok: [localhost] => {
            "msg": "15"
        }


## Example 3: Count List Items

        - name: Count the number of items in a list
        hosts: localhost
        gather_facts: no
        vars:
            fruits:
            - apple
            - banana
            - orange
        tasks:
            - name: Count items
            debug:
                msg: "Total fruits: {{ fruits | length }}"

**output**


        ok: [localhost] => {
            "msg": "Total fruits: 3"
        }

## Commonly Used Filters

|  Filters  |         Purpose            |          Example               |
| --------- | -------------------------- | ------------------------------ |
| `upper`   | Convert to UPPERCASE       | `"devops" \| upper` → `DEVOPS` |
| `lower`   | Convert to lowercase       | `"LINUX" \| lower` → `linux`   |
| `int`     | Convert to integer         | `"5" \| int` → `5`             |
| `length`  | Count items in list/string | `[1,2,3] \| length` → `3`      |
| `default` | Set fallback value         | `var \| default('N/A')`        |
| `replace` | Replace text               | `"abc" \| replace('a', 'x')`   |



# 2. Lookups (Fetch External Data)

**What are Lookups?**

Lookups are used to fetch data from external sources such as:

- environment variables

- files

- databases

- AWS metadata

- and more

Think of lookups as asking data from outside Ansible.

 Example 1: Get Your System Username

        {{ lookup('env', 'USER') }}

🖥 Output (on your machine):

        rahul


### Example 1: Read File Content

    - name: Read content from file
    hosts: localhost
    gather_facts: no
    tasks:
        - name: Show file content
        debug:
            msg: "{{ lookup('file', '/etc/hostname') }}"

### Example 2: Get Environment Variable

        - name: Get environment variable
        hosts: localhost
        gather_facts: no
        tasks:
            - name: Show current user
            debug:
                msg: "{{ lookup('env', 'USER') }}"


**Output** (depends on system)


### example:3

            ok: [localhost] => {
                "msg": "john"
            }

    - name: Practice filters and lookups
    hosts: localhost
    gather_facts: no
    vars:
        my_name: "ansible"
        score_1: "25"
        score_2: "30"
    tasks:
        - name: Show name in uppercase
        debug:
            msg: "{{ my_name | upper }}"

        - name: Add two scores
        debug:
            msg: "{{ score_1 | int + score_2 | int }}"

        - name: Show current home directory
        debug:
            msg: "{{ lookup('env', 'HOME') }}"


**Filters vs Lookups**

| Feature      |        Filters               |            Lookups                 |
| ----------- | ---------------------------- | ---------------------------------- |
|  Used for | Transforming data            | Fetching data from outside sources |
|  Example  | `"hello" \| upper` → `HELLO` | `lookup('env', 'USER')`            |
|  Used in  | Variables, tasks, templates  | Variables, tasks                   |
