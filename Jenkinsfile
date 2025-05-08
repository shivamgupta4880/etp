pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shivamgupta4880/etp:latest"
        REPO_URL = 'https://github.com/shivamgupta4880/etp.git'
        BRANCH = 'main'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                        bat "docker push ${DOCKER_IMAGE}"
                        bat "docker logout"
                    }
                }
            }
        }

        

        stage('Check Running Containers') {
            steps {
                bat 'docker ps'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
        
    }
}
