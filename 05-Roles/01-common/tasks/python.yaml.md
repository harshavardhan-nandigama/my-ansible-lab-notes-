

### Common Task: `python.yaml`

#### What This File Does

This task file automates the **setup of Python runtime and dependencies** for Python-based RoboShop components (for example: `payment` service).

✅ Installs required system packages for Python development
✅ Installs required Python libraries from `requirements.txt`

#### Task Breakdown

| Module | Purpose                                   |
| ------ | ----------------------------------------- |
| `dnf`  | Installs system-level Python dependencies |
| `pip`  | Installs project-specific Python packages |

#### Key Tasks

```yaml
- name: install python3 packages
  ansible.builtin.dnf:
    name: "{{ item }}"
    state: installed
  loop:
    - python3
    - gcc
    - python3-devel

- name: install dependencies
  ansible.builtin.pip:
    requirements: requirements.txt
    executable: pip3.9
  args:
    chdir: /app
```

#### Why This Is Useful

* Ensures **consistent Python runtime environment** across servers
* Automatically installs both:

  * system-level libraries (`gcc`, `python3-devel`)
  * application-level libraries (`Flask`, `pymysql`, etc. from `requirements.txt`)

#### Real-World Scenario

Imagine your team needs to add `requests` or `boto3` to the payment service:
✅ Just add it to `requirements.txt` →
✅ This playbook will auto-install it during next deployment.


