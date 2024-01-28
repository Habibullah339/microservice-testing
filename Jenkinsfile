pipeline {
    agent any
    
    environment {
        GITHUB_CREDENTIAL_ID = '33339'
        DOCKERHUB_CREDENTIAL_ID = '3338'  // Update with the actual credential ID
        DOCKER_IMAGE_NAME = 'habib339/sample-image'
        DOCKERHUB_USERNAME = 'habib339'  // Update with your Docker Hub username
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git credentialsId: GITHUB_CREDENTIAL_ID, url: 'https://github.com/Habibullah339/microservice-testing', branch: 'main'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "sudo docker build -t $DOCKER_IMAGE_NAME ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Use withCredentials to securely pass Docker Hub credentials
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'DOCKERHUB_CREDENTIAL_ID',
                            usernameVariable: 'DOCKERHUB_USERNAME',
                            passwordVariable: 'DOCKERHUB_PASSWORD'
                        )
                    ]) {
                        sh "docker login --username \${DOCKERHUB_USERNAME} --password \${DOCKERHUB_PASSWORD}"
                        sh "docker tag $DOCKER_IMAGE_NAME ${DOCKER_IMAGE_NAME}:latest"
                        sh "docker push ${DOCKER_IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    sh 'minikube start'
                    sh 'kubectl config use-context minikube'
                    sh 'kubectl apply -f your-path-to-kubernetes-manifests'
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'minikube stop'
            }
        }
    }
}
