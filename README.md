first-1 (azure devops)
https://aex.dev.azure.com/me?mkt=en-GB
# Second experiment yaml file:

```
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

stages:
  - stage: Build
    displayName: 'Build Stage'
    jobs:
      - job: BuildJob
        displayName: 'Build Job'
        steps:
          - script: |
              echo "Restoring project dependencies..."
            displayName: 'Restore dependencies'

          - script: |
              echo "Running unit tests..."
            displayName: 'Run unit tests'

  - stage: Test
    displayName: 'Test Stage'
    dependsOn: Build
    isSkippable: false
    jobs:
      - job: TestJob
        displayName: 'Test Job'
        steps:
          - script: |
              echo "Running unit tests..."
            displayName: 'Run unit tests'
```
# 🪟 **Procedure: Install Gradle on Windows (Exp 7)**

## Step 1: Install Java (JDK)

Gradle requires Java.

1. Download JDK from:
    [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/)

2. Install it normally.

3. Set **JAVA_HOME**:

   * Open **Environment Variables**
   * Under *System Variables* → Click **New**

     ```
     Variable Name: JAVA_HOME
     Variable Value: C:\Program Files\Java\jdk-XX
     ```

4. Add Java to PATH:

   ```
   %JAVA_HOME%\bin
   ```

### Verify:

Open CMD:

```cmd
java -version
```

---

## Step 2: Download Gradle

1. Go to:
    [https://gradle.org/releases/](https://gradle.org/releases/)

2. Download:
    **Binary-only ZIP**

---

##  Step 3: Extract Gradle

1. Extract ZIP file
2. Move folder to:

   ```
   C:\Gradle
   ```

 Final path example:

```
C:\Gradle\gradle-8.5
```

---

##  Step 4: Set Environment Variables

### Add GRADLE_HOME

* Variable Name:

```
GRADLE_HOME
```

* Variable Value:

```
C:\Gradle\gradle-8.5
```

---

### Add to PATH

Edit **Path** → Add:

```
%GRADLE_HOME%\bin
```

---

##  Step 5: Verify Installation

Open **Command Prompt**:

```cmd
gradle -v
```
You should see Gradle version info.

---

#  Step 6: Create Gradle Project

```cmd
mkdir HelloGradle
cd HelloGradle
gradle init
```

Select:

* Application
* Java
* Default options
* applicartion structure- 1.
* build script- Groovy
* test - Junit(4)

---

HelloGradle/
 ├── build.gradle
 ├── settings.gradle
 ├── gradlew
 ├── gradlew.bat
 └── src/
      └── main/java/org/example/App.java
---

# Step 7: Build & Run

```cmd
.\gradlew run
.\gradlew run
```

# Short Procedure (For Record)

1. Install Java JDK and set `JAVA_HOME`.
2. Download Gradle binary ZIP.
3. Extract it to `C:\Gradle`.
4. Set `GRADLE_HOME` and update `PATH`.
5. Verify using `gradle -v`.
6. Initialize project using `gradle init`.
7. Build and run using `gradlew build` and `gradlew run`.

---

# Common Errors

 `'gradle' is not recognized`
➡ Restart CMD / check PATH

 Wrong JAVA_HOME
➡ Verify path carefully

---

Got it  — here are the **clear, lab-ready steps for Ansible (Exp 6 + Exp 8)**. You can both **perform** and **write this in your record**.

---

#  **ANsible Procedure (Step-by-Step)**

## Step 1: Install Ansible (Control Node)

```bash
sudo apt update
sudo apt install ansible -y
```

###  Verify:

```bash
ansible --version
```

---

## Step 2: Setup SSH Between Nodes

### Generate SSH key (Control Node):

```bash
sudo apt install openssh-client -y
sudo systemctl start ssh
sudo systemctl status ssh
ip a
ping <ip>
ssh-keygen
```

### Copy key to Managed Node:

```bash
ssh-copy-id username@10.10.17.4(ip)
```

### Test connection:

```bash
ssh username@10.10.17.4
```

Should login without password

---

## Step 3: Create Inventory File

```bash
nano inventory.ini
```

Add:

```ini
[webservers]
10.10.17.4 ansible_user=username
```

---

## Step 4: Test Connection Using Ansible

```bash
ansible all -i inventory.ini -m ping
```

### Output:

```
pong
```

---

#  **Experiment 6: Playbook (Install Nginx)**

## Step 5: Create Playbook

```bash
nano webserver.yml
```

Paste:

```yaml
---
- name: Install Nginx
  hosts: webservers
  become: true

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

---

## Step 6: Run Playbook

```bash
ansible-playbook -i inventory.ini webserver.yml --ask-become-pass
```

---

## Step 7: Verify

```bash
ssh username@10.10.17.4
nginx -v
```

---

# **Experiment 8: Roles (Install Apache)**

##  Step 8: Create Role

```bash
ansible-galaxy init my_web_role
```

---

##  Step 9: Edit Role Tasks

```bash
nano my_web_role/tasks/main.yml
```

Paste:

```yaml
---
- name: Install Apache
  apt:
    name: apache2
    state: present
    update_cache: yes

- name: Start Apache
  service:
    name: apache2
    state: started
    enabled: yes
```

---

##  Step 10: Create Main Playbook

```bash
nano site.yml
```

Paste:

```yaml
---
- name: Configure Web Server
  hosts: webservers
  become: true

  roles:
    - my_web_role
```

---

##  Step 11: Run Role Playbook

```bash
ansible-playbook -i inventory.ini site.yml -K
```

---

##  Step 12: Verify Apache

```bash
ssh username@10.10.17.4
apache2 -v
```

OR open browser:

```
http://10.10.17.4
```

---

# **Short Procedure (For Record Writing)**

1. Install Ansible on control node.
2. Configure SSH between control and managed node.
3. Create inventory file listing target systems.
4. Test connectivity using Ansible ping module.
5. Create playbook to install web server (nginx).
6. Execute playbook using ansible-playbook command.
7. Create role using ansible-galaxy.
8. Define tasks in role and create main playbook.
9. Run playbook to configure Apache server.
10. Verify installation on managed node.

---

# **Final Result**

> Thus, Ansible is installed and playbooks and roles are successfully executed to configure web servers.

---

If you want next:
 Viva questions + answers
 Diagram (Control node ↔ Managed node)
Help debug your exact lab error

Just tell 



