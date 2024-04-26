pipeline{
    agent any
    stages{  
        stage("Code Checkout"){
            steps{
                git url:"https://github.com/DevOps-Playbbok/Capstone-Project-LAB.git", branch:"main"
            }
        }
        stage("Clone the code"){
            steps{
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
    }
