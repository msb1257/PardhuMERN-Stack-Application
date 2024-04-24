### In this LAB-3, we will discuss DevOps and DevSecOps tools:

### Tools Covered:
-  Linux
-  Git and GitHub
-  Docker
-  Docker-compose
-  Jenkins CI/CD
-  SonarQube Scan
-  SonarQube Quality Gates
-  Trivy 

#

## Pre-requisites to implement this project:

-  AWS EC2 instance (Ubuntu) with instance type t2.large and root volume 29GB.

-  Installation of JAVA
   ```bash
   sudo apt update
   sudo apt install fontconfig openjdk-17-jre
   java -version
   ```
   
-  Installation of Jenkins
   ```bash
   sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
   echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
   https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
   /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt-get update
   sudo apt-get install jenkins
   ```
-  Setup Jenkins
   ```bash
   Public-IP:8080 (Jenkins running)
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
-  Docker and docker-compose installled
   ```bash
    sudo apt-get update
    sudo apt-get install docker.io -y
    sudo apt-get install docker-compose -y
    sudo usermod -aG docker $USER
    sudo reboot
   ```

-  Trivy installation
   ```bash
   sudo apt-get install wget apt-transport-https gnupg lsb-release
   wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
   echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a 
   /etc/apt/sources.list.d/trivy.list
   sudo apt-get update
   sudo apt-get install trivy
   ```

- SonarQube Server installed
   ```bash
    docker run -itd --name sonarqube-server -p 9000:9000 sonarqube:lts-community
   ```
#
## Steps for Jenkins CI/CD:

1)  Access Jenkins UI and setup Jenkins

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/1eec417e-95ab-4497-ad31-443ecd6b999e)

#

2)  Plugins Installation:

    - Go to <b><i><u>Manage Jenkins</u></i></b>, click on <b><i><u>Plugins</u></i></b> and install all the plugins listed below, we will require for other tools integration:

        - SonarQube Scanner (Version2.16.1)
        - Sonar Quality Gates (Version1.3.1)
        - OWASP Dependency-Check (Version5.4.3)
        - Docker (Version1.5)
#

3) Go to SonarQube Server and create token

    - Click on <b><i><u> Administration </u></i></b> tab, then <b><i><u> Security </u></i></b>, then <b><i><u> Users </u></i></b> and create Token.
    -  Create a webhook to notify Jenkins that Quality gates scanning is done. (We will need this step later)

        - Go to SonarQube Server, then <b><i><u> Administration </u></i></b>, then <b><i><u> Configuration </u></i></b> and click on <b><i><u> Webhook </u></i></b>, add webhook in below <b>Format</b>:
        > http://<jenkins_url>:8080/sonarqube-webhook/
        
        Example: 
        
        ```bash
            http://34.207.58.19:8080/sonarqube-webhook/
        ```

        ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/b9ef2301-b8ff-46f4-a457-6345d5e2dab6)


        ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/08a33164-f6a6-4c5d-8a34-7091cf8a5745)

#

4) Go to Jenkins UI <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> Credentials </u></i></b> and add SonarQube Credentials.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/f6db72ec-7d8c-4f4c-ae7a-55d99dd20ce9)

#

5) Now, It's time to integrate SonarQube Server with Jenkins, go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> System </u></i></b> and look for <b><i><u> SonarQube Servers </u></i></b> and add SonarQube.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/54849cb2-fe56-4acd-972d-3057a0eb3deb)

#

6) Go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i><u> tools </u></i></b>, look for <b><i><u> SonarQube Scanner installations </u></i></b> and add SonarQube Scanner.

> Note: Add name as ```Sonar```

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/1fe926f6-a844-42d4-bce4-62193dde6640)

7) Integrate OWASP with Jenkins, go to <b><i><u> Manage Jenkins </u></i></b>, then <b><i> tools </i></b>, look for <b><i><u>Dependency-Check installations</u></i></b> and add Dependency-Check.

> Note: Add name as ```dc```

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/14516995-0c96-4110-bb96-97a37a9fe57d)

#

8) For trivy, we have already installed it, in pre-reuisites.

![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/0fcd1620-bd64-4286-bc13-f6652d4527c6)

#

9) Now, It's time to create a CI/CD pipeline in Jenkins:
    -  Click on <b><i><u>New Item</u></i></b> and give it a name and select <b><i><u>Pipeline</u></i></b>.
    -  Select <b><i><u>GitHub Project</u></i></b> and paste your GitHub repository link.
    -  Scroll down and in <b><i><u>Pipeline</u></i></b> section select <b><i><u>Pipeline script from SCM</u></i></b>, because our Jenkinsfile is present on GitHub.

    ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/39af1b22-28aa-4e36-b98c-0e7f120b5fbf)

    ![image](https://github.com/DevMadhup/node-todo-cicd/assets/121779953/b7153556-f847-40ee-9a98-ff3609930abd)
#

> Since our repository is private we need to create credentials for our repo. Click on Add Credentials --> Jenkins --> username as github usernmae and password as --> Personal access token

10) At last run the pipeline and after sometime your code is deployed using DevSecOps.




