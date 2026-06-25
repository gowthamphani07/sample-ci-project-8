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
JAVA_HOME:
C:\Program Files\Java\jdk-17
Maven
Name: Maven
MAVEN_HOME:
C:\apache-maven-3.9.9

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
