Step 9: Start Jenkins
Open browser:
http://localhost:8080
Login.

Step 10: Install Plugins

Go to:
Manage Jenkins
→ Plugins

Install:

Git Plugin
Maven Integration Plugin
Ansible Plugin
Pipeline Plugin

Restart Jenkins.

Step 11: Configure JDK and Maven

Go to:
Manage Jenkins
→ Global Tool Configuration

Add:

JDK
Name: JDK-17
JAVA_HOME:C:\Program Files\Java\jdk-17
Maven
Name: Maven
MAVEN_HOME:C:\apache-maven-3.9.9

Save.

Step 12: Create Jenkins Job
Dashboard
→ New Item

Name: Maven-CI-CD
Select:Freestyle Project
Click OK.

Step 13: Configure GitHub Repository

Source Code Management:Git
Repository URL:https://github.com/<username>/sample-ci-project-8.git
Branch:*/main

Step 14: Configure Build Trigger

Enable:Poll SCM
Schedule:H/5 * * * *

Step 15: Configure Build Step

Add Build Step:Invoke Top-Level Maven Targets
Goals:clean compile test package

Add Second Build Step

Execute Windows Batch Command

Command:
java -cp target\sample-ci-project-1.0-SNAPSHOT.jar com.multit.App

Save.

Step 16: Archive JAR

Post Build Action:Archive the Artifacts
Files:target/*.jar

Save.

Step 17: Build Jenkins Job

Click:

Build Now

Expected:BUILD SUCCESS

Step 18: Open Ubuntu WSL
mkdir ~/ansible-lab1
cd ~/ansible-lab1
Step 19: Create Inventory
nano inventory.ini

Paste:

[local]
localhost ansible_connection=local

Save:

CTRL+O
Enter
CTRL+X
Step 20: Create Deploy Playbook
nano deploy.yml

Paste:

---
- name: Deploy JAR
  hosts: local
  become: true

  tasks:

    - name: Copy JAR File
      copy:
        src: /mnt/c/ProgramData/Jenkins/.jenkins/workspace/Maven-CI-CD/target/sample-ci-project-1.0-SNAPSHOT.jar
        dest: /home/gowtham/ansible-lab1/app.jar
        mode: '0755'

    - name: Run JAR
      shell: nohup java -jar /home/gowtham/ansible-lab1/app.jar > /home/gowtham/ansible-lab1/app.log 2>&1 &

Save.

Step 21: Install Java in Ubuntu
sudo apt update
sudo apt install openjdk-17-jdk -y

Verify:

java -version
Step 22: Run Playbook
ansible-playbook -i inventory.ini deploy.yml --ask-become-pass

Enter Ubuntu password.

Expected:

PLAY RECAP

localhost
ok=3
changed=2
failed=0
Step 23: Verify Deployment

Check log:

cat app.log

Expected:

Hello Maven
This is the simple realworld example....
Sum of 5 and 10 is 15
