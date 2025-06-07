
# Shipping Component - Ansible Role

This Ansible role configures the `shipping` component of the Roboshop application.

## Component Type

- **Type:** Java (Maven-built) application
- **Dependencies:** MySQL (data import)

## Tasks Included

```yaml
- name: app setup
  include_role:
    name: common
    tasks_from: app-setup

- name: maven setup
  include_role:
    name: common
    tasks_from: maven

- name: import data
  community.mysql.mysql_db:
    name: all
    login_user: "{{ MYSQL_ROOT_USER }}"
    login_password: "{{ MYSQL_ROOT_PASSWORD }}"
    login_host: "{{ MYSQL_HOST }}"
    state: import
    target: "{{ item }}"
  loop:
  - /app/db/schema.sql
  - /app/db/app-user.sql
  - /app/db/master-data.sql

- name: systemd setup
  include_role:
    name: common
    tasks_from: systemd
````

## Files Used

* Common Role Tasks:

  * `common/tasks/app-setup.yaml`
  * `common/tasks/maven.yaml`
  * `common/tasks/systemd.yaml`
* Database SQL Files:

  * `/app/db/schema.sql`
  * `/app/db/app-user.sql`
  * `/app/db/master-data.sql`

## Variables Required

* `MYSQL_ROOT_USER`
* `MYSQL_ROOT_PASSWORD`
* `MYSQL_HOST`

## Usage

```yaml
- name: Configure Shipping Component
  hosts: shipping
  roles:
    - shipping
```

## Notes

* **Shipping** is a **Java application** built using Maven.
* After the app is built, required **SQL data is imported into MySQL**.
* Final step configures the systemd service for auto-start on boot.

```
