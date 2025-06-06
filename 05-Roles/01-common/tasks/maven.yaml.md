

## Common Tasks: maven.yaml

This task file is designed for Java-based Roboshop components that use Maven for building the application and Python libraries for certain dependencies.

### Tasks Explained:

1. **Install Maven and MySQL client:**

   ```yaml
   - name: install maven and mysql
     ansible.builtin.dnf:
       name: "{{ item }}"
       state: present
     loop:
     - maven
     - mysql
   ```

   * Installs Maven for building Java projects.
   * Installs MySQL client libraries required for connecting to MySQL databases.
   * Both packages are installed using the system’s package manager `dnf`.

2. **Install Python packages (pymysql and cryptography):**

   ```yaml
   - name: install pymysql and cryptography
     ansible.builtin.pip:
       name: "{{ item }}"
       executable: pip3.9
     loop:
     - cryptography
     - pymysql
   ```

   * Installs Python packages required by the application.
   * `pymysql` allows Python code to interact with MySQL.
   * `cryptography` provides secure encryption and decryption functionality.
   * Installed using pip for Python 3.9.

3. **Build Maven dependencies and package application:**

   ```yaml
   - name: install maven dependencies
     ansible.builtin.command: mvn clean package
     args:
       chdir: /app
   ```

   * Runs Maven’s `clean package` command inside the `/app` directory.
   * This compiles the Java code, runs tests, and packages the application into a `.jar` file.

4. **Rename the generated JAR file:**

   ```yaml
   - name: rename jar file
     ansible.builtin.command: "mv target/{{ component }}-1.0.jar {{ component }}.jar"
     args:
       chdir: /app
   ```

   * Renames the JAR file generated by Maven to a simplified name.
   * Uses the `{{ component }}` variable to dynamically handle different components.
   * Moves the file from `target/` directory to `/app` root with a consistent naming pattern.

---

### Summary

This reusable role helps Java microservices by installing necessary build tools and dependencies, compiling the application, and preparing the JAR file for deployment. It simplifies Maven-based builds while integrating Python dependencies required by the application.

