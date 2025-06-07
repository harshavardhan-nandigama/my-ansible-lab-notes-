

##  Catalogue Role — Tasks Explanation

This role automates the installation and configuration of the **Catalogue** microservice for the RoboShop project.

---

### Tasks Breakdown

---

### 1️⃣ App Setup (Common Role)

```yaml
- name: app setup
  include_role:
    name: common
    tasks_from: app-setup
```

* Reuses the `common` role `app-setup.yaml`.
* Prepares `/app` directory.
* Creates roboshop system user.
* Downloads and extracts Catalogue app code.

---

### 2️⃣ NodeJS Setup (Common Role)

```yaml
- name: nodejs setup
  include_role:
    name: common
    tasks_from: nodejs
```

* Reuses `common` role `nodejs.yaml`.
* Installs NodeJS 20.
* Installs required NPM dependencies for Catalogue app.

---

### 3️⃣ Copy MongoDB Repo

```yaml
- name: copy mongodb repo
  ansible.builtin.copy:
    src: mongo.repo
    dest: /etc/yum.repos.d/mongo.repo
```

* Adds MongoDB YUM repo to install the MongoDB CLI client (mongosh).

---

### 4️⃣ Install MongoDB Client

```yaml
- name: install mongodb client
  ansible.builtin.dnf:
    name: mongodb-mongosh
    state: present
```

* Installs `mongosh`, required to interact with MongoDB server.

---

### 5️⃣ Check Products Loaded or Not

```yaml
- name: check products loaded or not
  ansible.builtin.command: mongosh --host mongodb.harshavn24.site --eval 'db.catalogue.products.count()'
  register: catalogue_output
```

* Executes a MongoDB query to check whether products data is already loaded.
* Saves the result in `catalogue_output`.

---

### 6️⃣ Print Catalogue Output (For Debugging)

```yaml
- name: print catalogue output
  ansible.builtin.debug:
    msg: "{{ catalogue_output }}"
```

* Debug step: prints how many products were found in the database.

---

### 7️⃣ Load Products (If Not Loaded)

```yaml
- name: load products
  ansible.builtin.shell: mongosh --host mongodb.harshavn24.site < /app/db/master-data.js
  when: catalogue_output.stdout_lines[-1] | int == 0
```

* If products count < 0 (or failed), loads product data using `master-data.js`.
* Ensures MongoDB is populated with initial data for Catalogue service.

---

### 8️⃣ SystemD Setup (Common Role)

```yaml
- name: systemd setup
  include_role:
    name: common
    tasks_from: systemd.yaml
```

* Reuses `common` role `systemd.yaml`.
* Deploys and manages the Catalogue SystemD service.
* Enables the service to auto-start and restarts it.

---

### Summary:

✅ App code setup
✅ NodeJS setup
✅ MongoDB CLI installation
✅ Product data check and load
✅ SystemD service management

