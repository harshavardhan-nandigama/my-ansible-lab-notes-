

### Explanation of the Payment Component Playbook

This playbook sets up the **payment service** by reusing common tasks defined in shared roles. It breaks down the setup into three key steps:

1. **App Setup**

   ```yaml
   - name: app setup 
     include_role: 
       name: common
       tasks_from: app-setup
   ```

   This step runs the `app-setup` tasks from the `common` role. It typically includes:

   * Creating the `/app` directory.
   * Creating the system user `roboshop`.
   * Downloading and extracting the payment application code into `/app`.

2. **Python Setup**

   ```yaml
   - name: python setup
     include_role: 
       name: common
       tasks_from: python
   ```

   This step installs Python and Python-related dependencies needed by the payment service. It:

   * Installs required Python packages (`python3`, `gcc`, `python3-devel`).
   * Installs Python libraries from the application's `requirements.txt` file using `pip`.

3. **Systemd Setup**

   ```yaml
   - name: systemd setup
     include_role: 
       name: common
       tasks_from: systemd
   ```

   This step configures and manages the payment service as a systemd service. It:

   * Copies the payment service systemd unit file to the appropriate system directory.
   * Reloads the systemd daemon to recognize the new service.
   * Enables and starts the payment service to run automatically on system boot.

---


