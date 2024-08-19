pipeline {
    agent any
    environment {
        SONAR_HOME = tool "Sonar"
    }
    stages {
        stage("Code Checkout"){ 
        steps{
            git(
                url: "https://github.com/amigo-nishant/Capstone_Project_Clone.git",
                branch: "main",
                credentialsId: "jenkins-secret",
                changelog: true,
                poll: true
            )
        }
    }
        stage("SonarQube Code Analysis") {
        steps {
            withSonarQubeEnv("Sonar") {
                sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=airbyte -Dsonar.projectKey=airbyte"
            }
        }
    }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs --format  table -o trivy-fs-report.html ."
            }
        }
        stage("OWASP Dependency Check"){
            steps{
                echo "Skipping due to some dependency test cases are yet to be merged to master"
                // dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
                // dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("PROD Deployment"){
            steps{
              echo "Test Cases check" 
              echo "PROD Deployment successfull" 
            }
         }
      }
   }
