

### main.yaml (Roles Playbook)

```yaml
- name: configure {{ component }}
  become: yes
  hosts: "{{ component }}"
  roles: # this will automatically load folder called roles in current working directory.
  - "{{ component }}"
```

#### Explanation:

✅ **Dynamic component configuration:**

* `component` is passed as an extra variable (`-e "component=catalogue"`)
* The play runs only for the corresponding host group (defined in `inventory.ini`)

✅ **Roles-based architecture:**

* Ansible will load `roles/{{ component }}/` folder.
* The role will contain:

  * `tasks/main.yaml` → where your actual steps are written
  * optionally: `files/`, `handlers/`, `defaults/`, `templates/`

✅ **Why this is useful:**
👉 Clean separation of logic
👉 Code reuse
👉 Easy to maintain
👉 You don’t repeat the same steps in multiple playbooks
👉 Only 1 main.yaml — same for all components!

✅ **How to run:**

```bash
ansible-playbook -i inventory.ini -e "component=catalogue" main.yaml
ansible-playbook -i inventory.ini -e "component=user" main.yaml
ansible-playbook -i inventory.ini -e "component=shipping" main.yaml
```
