

### Common Task: `systemd.yaml`

#### What This File Does

This task file automates the **setup and management of systemd service** for the given RoboShop component.

✅ Deploys the systemd service unit
✅ Reloads systemd daemon to recognize new unit
✅ Starts and enables the service

#### Task Breakdown

| Module     | Purpose                                     |
| ---------- | ------------------------------------------- |
| `template` | Deploys systemd service file with variables |
| `systemd`  | Reloads systemd to apply changes            |
| `service`  | Starts and enables service                  |

#### Key Tasks

```yaml
- name: copy {{ component }} service to system directory
  ansible.builtin.template:
    src: "{{ component }}.service.j2"
    dest: "/etc/systemd/system/{{ component }}.service"

- name: systemctl daemon reload
  ansible.builtin.systemd:
    daemon_reload: yes

- name: start and enable {{ component }}
  ansible.builtin.service:
    name: "{{ component }}"
    state: restarted
    enabled: yes
```

#### Why This Is Useful

* Standardizes service management for all components:

  * Catalogue
  * User
  * Cart
  * Shipping
  * Payment
  * etc.

* You don’t have to write separate tasks for each component → just reuse this task.

* Uses `template` instead of `copy` → supports **Jinja2 templating** → dynamic & flexible.

* Restarts the service even if it is not running → safe behavior for idempotent playbooks.

#### Real-World Scenario

✅ Developer team changes `/app/user/index.js` and commits update
✅ You run playbook → service auto-restarted → no manual SSH needed

✅ CI/CD Pipeline can safely run playbooks → services always restarted properly.
