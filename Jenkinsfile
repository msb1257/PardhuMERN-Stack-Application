pipeline {
    agent any
    stages {
        stage("Clone Git Repository") {
            steps {
                git(
                    url: "https://github.com/DevOps-Playbbok/Capstone-Project-LAB.git",
                    branch: "main",
                    credentialsId: "jenkins-git",
                    changelog: true,
                    poll: true
                    )
            }
        }
    }
}
