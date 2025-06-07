

### Frontend Component Playbook — Explanation

This playbook configures the **Frontend (UI)** of the **RoboShop Project**.
It installs and configures **Nginx** to serve the frontend static content.

---

### Step-by-step Breakdown:

---

### 1️⃣ Manage Nginx module version

```yaml
- name: disable default nginx
- name: enable nginx
```

👉 This ensures you use the required Nginx version:

* Disables the default Nginx module.
* Enables Nginx version **1.24** which is a modern, stable version.

---

### 2️⃣ Install & Start Nginx

```yaml
- name: install nginx
- name: start and enable nginx
```

👉 Installs Nginx using `dnf`, starts the service, and enables it to run on boot.

---

### 3️⃣ Prepare Nginx Web Directory

```yaml
- name: remove html directory
- name: create html directory
```

👉 Cleans and recreates `/usr/share/nginx/html`:

* This ensures the directory is empty before deploying the latest frontend code.

---

### 4️⃣ Download and Deploy Frontend Code

```yaml
- name: download frontend code
- name: unzip frontend code
```

👉 Downloads the **frontend-v3.zip** artifact from S3 and extracts it into `/usr/share/nginx/html` — this is the directory served by Nginx.

---

### 5️⃣ Configure Nginx

```yaml
- name: remove default nginx conf
- name: copy roboshop nginx conf
```

👉 Removes the default Nginx config and replaces it with a **custom RoboShop configuration** using a Jinja2 template `nginx.conf.j2`.

The new config is optimized for serving the RoboShop frontend and correctly sets up **backend service reverse proxies**, if needed.

---

### 6️⃣ Notify to Restart Nginx (if config changed)

```yaml
notify:
  - Restart nginx
```

👉 If the new Nginx config is deployed, this triggers a **handler** to restart Nginx and apply the configuration.

---

### Summary:

This playbook fully automates the **frontend setup**:
✅ Installs and configures Nginx
✅ Deploys the latest frontend code
✅ Applies a custom Nginx configuration
✅ Ensures the frontend runs reliably on system boot

---

### Why this is good practice:

✅ Version-controlled setup of the web server
✅ Clean deploy of the latest frontend version
✅ Idempotent — you can safely re-run this playbook any time
✅ Uses **handlers** for efficient service restarts only when needed

