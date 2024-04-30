pipeline{
    agent any
    environment{
        SONAR_HOME= tool "Sonar"
    }
    stages{ 
        stage("Code Checkout"){
            steps{
              git(
                 url: "https://github.com/DevOps-Playbbok/Capstone-Project-LAB.git",
                 branch: "main",
                 credentialsId: "jenkins-git",
                 changelog: true,
                 poll: true
                 )
            }
        }
        stage("Code Deploy: Docker-compose"){
            steps{    
               
                sh "docker ps"
            }
        }
    }
}
