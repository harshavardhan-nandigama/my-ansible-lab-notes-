

## Common Tasks: app-setup.yaml

This playbook snippet contains common tasks that are shared across multiple Roboshop components. It handles the setup of the application environment on the target server.

### Tasks Explained:

1. **Create application directory:**

   ```yaml
   - name: create app directory
     ansible.builtin.file:
       path: /app
       state: directory
   ```

   * Ensures the directory `/app` exists on the target machine.
   * This directory will hold the application code for the component.

2. **Create Roboshop system user:**

   ```yaml
   - name: create roboshop system user
     ansible.builtin.user:
       name: roboshop
       shell: /sbin/nologin
       system: true
       home: /app
   ```

   * Creates a system user named `roboshop`.
   * The user’s shell is set to `/sbin/nologin` to prevent direct login.
   * The home directory is set to `/app`.
   * This user owns and runs the application code securely.

3. **Download the component code:**

   ```yaml
   - name: "download {{ component }} code"
     ansible.builtin.get_url:
       url: "https://roboshop-artifacts.s3.amazonaws.com/{{ component }}-v3.zip"
       dest: /tmp/{{ component }}.zip
   ```

   * Downloads the zipped source code for the specific component from the Roboshop artifact repository.
   * The `{{ component }}` variable dynamically replaces with the component’s name (e.g., `user`, `catalogue`).
   * The zip file is saved to the `/tmp` directory.

4. **Extract the component code:**

   ```yaml
   - name: "extract {{ component }} code"
     ansible.builtin.unarchive:
       src: "/tmp/{{ component }}.zip"
       dest: /app
       remote_src: yes
   ```

   * Extracts the downloaded zip archive to the `/app` directory.
   * `remote_src: yes` indicates that the archive is already on the remote machine (not local to the control node).

---

### Summary

This common role setup is reusable for any Roboshop microservice by simply passing the component name as a variable. It automates the creation of the app directory, system user, downloads, and extracts the application code, laying the foundation for further service-specific configurations.

