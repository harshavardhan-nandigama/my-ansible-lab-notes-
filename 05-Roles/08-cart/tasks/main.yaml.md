
# Cart Component - Ansible Role

This Ansible role configures the `cart` component of the Roboshop application.

## Component Type

- **Type:** NodeJS application

## Tasks Included

```yaml
- name: app setup
  include_role:
    name: common
    tasks_from: app-setup

- name: nodejs setup
  include_role:
    name: common
    tasks_from: nodejs

- name: systemd setup
  include_role:
    name: common
    tasks_from: systemd.yaml
````

## Files Used

* Common Role Tasks:

  * `common/tasks/app-setup.yaml`
  * `common/tasks/nodejs.yaml`
  * `common/tasks/systemd.yaml`

## Usage

```yaml
- name: Configure Cart Component
  hosts: cart
  roles:
    - cart
```

## Notes

* This role uses **Ansible roles** and **includes common reusable tasks**.
* The unnecessary MySQL root password setup task has been removed, as it belongs to the MySQL role.
* The `cart` service connects to Redis and MySQL (configured in their respective roles).

