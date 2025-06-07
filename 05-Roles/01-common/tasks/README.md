

# Common Ansible Tasks for RoboShop Project

This folder contains **common reusable Ansible task files** used across multiple RoboShop components.
These tasks simplify the playbooks by avoiding repetition and keeping the structure clean and modular.

## Why This Folder Is Useful

✅ Promotes **DRY** (Don't Repeat Yourself) principle
✅ Standardizes component setup
✅ Easy to maintain → one place to update repeated logic
✅ Simplifies main role files and playbooks

## Common Task Files

| File Name        | Purpose                                                                      |
| ---------------- | ---------------------------------------------------------------------------- |
| `app-setup.yaml` | Creates `/app` directory, roboshop user, downloads & extracts component code |
| `maven.yaml`     | Installs Maven, builds Java apps, prepares JAR files                         |
| `nodejs.yaml`    | Installs Node.js 20, installs app dependencies                               |
| `python.yaml`    | Installs Python 3 and dependencies via `pip`                                 |
| `systemd.yaml`   | Sets up and manages systemd services for components                          |

## How to Use

In your main role `tasks/main.yaml`, simply include:

```yaml
- name: app setup
  ansible.builtin.import_tasks: common/app-setup.yaml
  vars:
    component: user
```

Similarly:

```yaml
- name: setup systemd
  ansible.builtin.import_tasks: common/systemd.yaml
  vars:
    component: payment
```

## Real-World Scenario

✅ All microservices (user, cart, payment, shipping, etc.) share similar steps → **one common task** is used for all → reduces errors and duplication.

✅ If tomorrow you want to change `/app` directory permission or `roboshop` user → update only in `common/app-setup.yaml`, and all components will pick up the change.
