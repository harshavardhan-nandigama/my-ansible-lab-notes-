
# User Component - Ansible Role

This Ansible role configures the `user` component of the Roboshop application.

## Component Type

- **Type:** NodeJS application

## Tasks Included

1. **App Setup**
    - Creates `/app` directory
    - Creates `roboshop` system user
    - Downloads and extracts `user` component code

2. **NodeJS Setup**
    - Installs NodeJS v20
    - Installs NodeJS dependencies (via npm)

3. **SystemD Setup**
    - Installs systemd service for `user`
    - Enables and starts the `user` service

## Files Used

- Common Role:
    - `common/tasks/app-setup.yaml`
    - `common/tasks/nodejs.yaml`
    - `common/tasks/systemd.yaml`

## Usage

```yaml
- name: Configure User Component
  hosts: user
  roles:
    - user
````

## Notes

* Follows best practices using **common roles** for reusability.
* Clean and simple role structure.

