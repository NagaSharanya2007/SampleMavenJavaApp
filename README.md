SE LAB EXAM — EXACT STEP-BY-STEP PROCEDURE

Follow this from the beginning. Do not skip steps. This is based directly on your uploaded Set-2 question paper, including Maven, Git/GitHub, Docker, Tomcat, Ubuntu and Docker Hub. 


---

PART 1 — CLONE THE PROJECT

STEP 1 — Open Git Bash

Open Git Bash.

Go to the folder where you want the project:

cd ~/git

STEP 2 — Clone the repository

For OLES:

git clone git@github.com:archanareddyse/Lab-Internal-1-OLES.git

STEP 3 — Enter the project

cd Lab-Internal-1-OLES

STEP 4 — Check the files

ls

You should find:

pom.xml
src


---

PART 2 — CHECK MAVEN PROJECT

STEP 5 — Check Java

java -version

STEP 6 — Check Maven

mvn -version

STEP 7 — Check project structure

ls

Make sure:

pom.xml
src/

are present.


---

PART 3 — CREATE/FIX pom.xml

Open:

pom.xml

For the Java web application, make sure it contains:

<packaging>war</packaging>

Use Java 17:

<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>

Add Servlet API:

<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
</dependency>

Add JSTL:

<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>

Add JUnit:

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>

Save pom.xml.


---

PART 4 — BUILD MAVEN PROJECT

STEP 8 — Clean and build

mvn clean package

Wait for:

BUILD SUCCESS

STEP 9 — Check WAR

ls target

You should see something like:

LMS.war

or:

exam-system-1.0-SNAPSHOT.war

Remember the exact WAR filename.


---

PART 5 — IF MAVEN JAVA VERSION ERROR OCCURS

The question specifically gives the situation Java 17 project + Java 21 installed. 

Check Maven's Java:

mvn -version

Set POM to Java 17:

<maven.compiler.source>17</maven.compiler.source>
<maven.compiler.target>17</maven.compiler.target>

Build again:

mvn clean package

If you need detailed debugging:

mvn clean package -X


---

PART 6 — DEPENDENCY PROBLEM

If a dependency/class cannot be found:

STEP 10 — Check dependency tree

mvn dependency:tree

STEP 11 — Check local Maven repository

On Windows:

dir %USERPROFILE%\.m2\repository

On Git Bash/Linux:

ls ~/.m2/repository

For a specific dependency, navigate into its group/artifact/version folders and check for the .jar.


---

PART 7 — JUNIT TESTING

STEP 12 — Run tests

mvn test

STEP 13 — Test reports

Go to:

target/surefire-reports/

STEP 14 — Run one particular test class

mvn -Dtest=TestClassName test

Example:

mvn -Dtest=LoginTest test

STEP 15 — Run only failed tests

First run:

mvn test

Check:

target/surefire-reports/

Identify failed test classes and rerun them using:

mvn -Dtest=FailedTestClass test

For multiple failed classes:

mvn -Dtest=Test1,Test2,Test3 test

The question's testing tasks are on page 2 of the paper. 


---

PART 8 — RUN OLES IN ECLIPSE + TOMCAT

STEP 16 — Import Maven project

In Eclipse:

File
↓
Import
↓
Maven
↓
Existing Maven Projects
↓
Next

Select the cloned project folder.

Select:

pom.xml

Click:

Finish

STEP 17 — Add Tomcat

In Eclipse:

Window
↓
Show View
↓
Servers

If there is no Tomcat:

New Server
↓
Apache
↓
Tomcat v9.0

Select your Tomcat installation.

STEP 18 — Add OLES to Tomcat

Right-click:

Tomcat v9.0 Server at localhost

Choose:

Add and Remove...

Select:

OLES

Click:

Add >

Then:

Finish

STEP 19 — Start Tomcat

Right-click:

Tomcat v9.0 Server at localhost

Click:

Start

STEP 20 — Open application

If Tomcat uses port 8083 and your context root is OLES:

http://localhost:8083/OLES/


---

PART 9 — GIT WORKFLOW

The question specifically asks for SSH clone, status, remote, branch creation, commit, pull/rebase, rebase, revert, untracking a file, comparison, merge and push. 

STEP 21 — Check branch

git branch

STEP 22 — Check status

git status

STEP 23 — Check remote

git remote -v

STEP 24 — Create feature branch and switch to it

git switch -c feature/homepage

Check:

git branch

You should see:

* feature/homepage


---

PART 10 — MAKE CHANGES + COMMIT

STEP 25 — Modify the project

Change something in the project.

Create:

README.md

STEP 26 — Check changes

git status

STEP 27 — Add changes

git add .

STEP 28 — Commit

git commit -m "Update homepage"


---

PART 11 — GET LATEST REMOTE CHANGES WITHOUT LOSING YOUR COMMIT

While on:

feature/homepage

run:

git pull --rebase origin main

If conflicts occur:

git status

Fix the conflicted files.

Then:

git add .

Continue:

git rebase --continue


---

PART 12 — REBASE FEATURE BRANCH

To bring your feature branch up to date with main:

git switch feature/homepage

Then:

git rebase main

If conflicts occur:

git status

Fix files.

Then:

git add .
git rebase --continue


---

PART 13 — UNDO A COMMIT WITHOUT DELETING HISTORY

The question asks to undo the effect of an unwanted commit without deleting the commit from Git history.

Use:

git revert <commit-id>

Then:

git push


---

PART 14 — REMOVE FILE FROM GIT BUT KEEP IT ON COMPUTER

Suppose you accidentally committed:

secret.txt

Use:

git rm --cached secret.txt

Then:

git commit -m "Remove secret file from tracking"

The file remains on your computer but is removed from Git tracking.


---

PART 15 — COMPARE FEATURE BRANCH WITH MAIN

See file differences:

git diff main..feature/homepage

See commits that differ:

git log main..feature/homepage


---

PART 16 — MERGE FEATURE INTO MAIN

Switch to main:

git switch main

Update main:

git pull

Merge feature:

git merge feature/homepage

Check:

git status

Push main:

git push origin main

Verify remote:

git remote -v


---

PART 17 — DOCKER: CREATE DOCKERFILE

Go into your Maven project:

cd Lab-Internal-1-OLES

Make sure you have:

pom.xml
src/

Create a file named exactly:

Dockerfile

Put this inside:

FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package

FROM tomcat:9.0

COPY --from=build /app/target/LMS.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]

If your WAR is not named LMS.war, replace LMS.war with your actual WAR filename.

The question requires the Dockerfile to include a Maven/Java base image, working directory, project copy, Maven build, Tomcat, WAR deployment, port exposure and Tomcat startup. 


---

PART 18 — BUILD DOCKER IMAGE

From the folder containing Dockerfile:

docker build -t library-lms:1.0 .

Wait for the build to finish.

Verify:

docker images

Look for:

library-lms
1.0


---

PART 19 — RUN YOUR DOCKER IMAGE

Run:

docker run -d --name library-lms-container -p 7070:8080 library-lms:1.0

Check:

docker ps

Open browser:

http://localhost:7070

Because the Dockerfile copied the WAR as:

ROOT.war

the application is at:

http://localhost:7070/


---

PART 20 — CHECK DOCKER LOGS

If the application doesn't open:

docker logs library-lms-container

If you want live logs:

docker logs -f library-lms-container


---

PART 21 — PULL OFFICIAL TOMCAT

The exam separately asks you to pull Tomcat and run it as a container. 

docker pull tomcat:9.0

Check:

docker images


---

PART 22 — RUN PLAIN TOMCAT CONTAINER

docker run -d --name tomcat-server -p 7070:8080 tomcat:9.0

Check:

docker ps

Open:

http://localhost:7070


---

PART 23 — DEPLOY YOUR MAVEN WAR INTO TOMCAT CONTAINER

First build the WAR:

mvn clean package

Suppose it creates:

target/LMS.war

Copy it into the running Tomcat container:

docker cp target/LMS.war tomcat-server:/usr/local/tomcat/webapps/

Verify:

docker exec tomcat-server ls /usr/local/tomcat/webapps

You should see:

LMS.war

and after Tomcat deploys it, typically:

LMS/

Open:

http://localhost:7070/LMS

This manual WAR deployment is specifically required in Q5. 


---

PART 24 — UBUNTU + PYTHON

STEP 1 — Pull Ubuntu

docker pull ubuntu

STEP 2 — Create and run container

docker run -dit --name python-container ubuntu

STEP 3 — Check

docker ps

STEP 4 — Enter container

docker exec -it python-container bash

STEP 5 — Update packages

Inside Ubuntu:

apt update

STEP 6 — Install Python

apt install -y python3

STEP 7 — Check Python

python3 --version

STEP 8 — Start Python

python3

STEP 9 — Run program

print("Hello from Docker")

STEP 10 — Exit Python

exit()

STEP 11 — Exit Ubuntu

exit

These are the exact Ubuntu/Python tasks from Q6. 


---

PART 25 — PUSH YOUR CUSTOM IMAGE TO DOCKER HUB

Suppose:

Docker Hub username = YOUR_USERNAME
Repository = library-lms
Image = library-lms:1.0

STEP 1 — Login

docker login

STEP 2 — Tag your image

docker tag library-lms:1.0 YOUR_USERNAME/library-lms:1.0

Example:

docker tag library-lms:1.0 nagasharanya07/library-lms:1.0

STEP 3 — Check image

docker images

STEP 4 — Push image

docker push YOUR_USERNAME/library-lms:1.0

Example:

docker push nagasharanya07/library-lms:1.0

STEP 5 — Open Docker Hub

Go to your Docker Hub repository:

YOUR_USERNAME/library-lms

Check that:

1.0

appears as the image tag.

The paper specifically asks you to tag, log in, push and verify the image in Docker Hub. 


---

PART 26 — IF THE QUESTION CHANGES WAR TO JAR

The paper asks this separately. 

Change:

<packaging>war</packaging>

to:

<packaging>jar</packaging>

Configure an executable-JAR plugin with the main class, then:

mvn clean package

The output will be a:

target/*.jar


---

🔥 FULL EXAM COMMANDS — IN ORDER

If you want one block to memorize, read this:

# CLONE
git clone git@github.com:archanareddyse/Lab-Internal-1-OLES.git

cd Lab-Internal-1-OLES

ls

# MAVEN
java -version
mvn -version

mvn clean package

# TEST
mvn test

# GIT
git status
git remote -v
git branch
git switch -c feature/homepage
git status
git add .
git commit -m "Update homepage"

git pull --rebase origin main

git rebase main

git revert <commit-id>

git rm --cached <filename>
git add .
git commit -m "Remove file from tracking"

git diff main..feature/homepage
git log main..feature/homepage

git switch main
git pull
git merge feature/homepage
git push origin main

# DOCKER BUILD
docker build -t library-lms:1.0 .

docker images

# RUN CUSTOM IMAGE
docker run -d --name library-lms-container -p 7070:8080 library-lms:1.0

docker ps

docker logs library-lms-container

# TOMCAT
docker pull tomcat:9.0

docker run -d --name tomcat-server -p 7070:8080 tomcat:9.0

docker ps

# DEPLOY WAR
mvn clean package

docker cp target/LMS.war tomcat-server:/usr/local/tomcat/webapps/

docker exec tomcat-server ls /usr/local/tomcat/webapps

# UBUNTU + PYTHON
docker pull ubuntu

docker run -dit --name python-container ubuntu

docker exec -it python-container bash

apt update

apt install -y python3

python3 --version

python3

print("Hello from Docker")

exit()

exit

# DOCKER HUB
docker login

docker tag library-lms:1.0 YOUR_USERNAME/library-lms:1.0

docker push YOUR_USERNAME/library-lms:1.0

Browser URLs

Eclipse Tomcat if port = 8083:

http://localhost:8083/OLES/

Docker custom image:

http://localhost:7070/

Plain Tomcat + LMS.war:

http://localhost:7070/LMS

Plain Tomcat root:

http://localhost:7070/
