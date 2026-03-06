pipeline {
    agent any
    triggers { 
      githubPush() 
   }
    environment {
        DOCKER_IMAGE_NAME = 'calculator'
        GITHUB_REPO_URL = 'https://github.com/VarunreddyChintha/Checking-Jenkins.git'
        IMAGE_NAME='varun000reddy/calculator'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the GitHub repository
                    git branch: 'main', url: "https://github.com/VarunreddyChintha/Checking-Jenkins.git"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build("${DOCKER_IMAGE_NAME}", '.')
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                script{
                    docker.withRegistry('', 'DockerHubCred') {
                    sh 'docker tag calculator varun000reddy/calculator:latest'
                    sh 'docker push varun000reddy/calculator'
                    }
                 }
            }
        }

   stage('Run Ansible Playbook') {
            steps {
                script {
                    ansiblePlaybook(
                        playbook: 'deploy.yml',
                        inventory: 'inventory'
                     )
                }
            }
        }

    }
      post {
        always {
            mail(
                to: 'varun000reddy@gmail.com',
                subject: "Build ${currentBuild.currentResult}",
                body: "Build URL: ${env.BUILD_URL}"
            )
        }
    }
}
