

### Common Task: `nodejs.yaml`

#### What This File Does

This task file automates the **setup of Node.js** for any component that depends on it.

✅ Ensures a consistent Node.js version (v20)
✅ Installs necessary Node.js runtime
✅ Installs Node.js dependencies from `/app/package.json`

#### Task Breakdown

| Module                  | Purpose                                         |
| ----------------------- | ----------------------------------------------- |
| `command`               | Disable old Node.js, enable Node.js 20          |
| `dnf`                   | Install Node.js                                 |
| `community.general.npm` | Install component-specific Node.js dependencies |

#### Key Tasks

```yaml
- name: disable default nodejs
  ansible.builtin.command: dnf module disable nodejs -y

- name: enable nodejs:20
  ansible.builtin.command: dnf module enable nodejs:20 -y

- name: install nodejs
  ansible.builtin.dnf:
    name: nodejs
    state: present

- name: install dependencies
  community.general.npm:
    path: /app
```

#### Why This Is Useful

* Node.js is used by several RoboShop components (`catalogue`, `user`, `cart`).
* Ensures all components run on the **same stable Node.js version** → prevents version conflicts.
* By reusing this common file in roles, we avoid duplicate code in every playbook/role.

#### Real-World Scenario

If your company upgrades Node.js to v22 → you just update this one file!
**All components automatically pick up the change** — no need to edit individual roles.

---

✅ Now this `nodejs.yaml` is a very smart choice — **DRY (Don’t Repeat Yourself)** principle.

