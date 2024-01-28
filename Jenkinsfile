pipeline {
    agent any
    
    environment {
        GITHUB_CREDENTIAL_ID = '33339'
        DOCKERHUB_CREDENTIAL_ID = '5555'  // Update with the actual credential ID
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
                    // Use withDockerRegistry to authenticate and push Docker image
                    withDockerRegistry([credentialsId: DOCKERHUB_CREDENTIAL_ID, url: 'https://index.docker.io/v1/']) {
                        // This step will automatically handle login and logout
                        sh "docker tag $DOCKER_IMAGE_NAME ${DOCKER_IMAGE_NAME}:latest"
                        
                        // Use --password-stdin to securely pass the DockerHub access token
                        sh "echo \$(dockerhub_accesstoken) | docker login --username $DOCKERHUB_USERNAME --password-stdin"
                        
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
