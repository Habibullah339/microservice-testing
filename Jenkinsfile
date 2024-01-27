pipeline {
    agent any
    
    environment {
        // Set your credentials IDs
        GITHUB_CREDENTIAL_ID = '33339'
        DOCKERHUB_CREDENTIAL_ID = '3338'
        DOCKER_IMAGE_NAME = 'habib339/sample-image'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the GitHub repository
                    git credentialsId: GITHUB_CREDENTIAL_ID, url: 'https://github.com/Habibullah339/microservice-testing', branch: 'main'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "sudo docker build -t $DOCKER_IMAGE_NAME ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Log in to DockerHub
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIAL_ID, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                       // sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                       // Create a Docker configuration file for secure login
                        def dockerConfig = """{
                            "auths": {
                                "https://index.docker.io/v1/": {
                                    "auth": "${DOCKER_USERNAME}:${DOCKER_PASSWORD.encodeBase64()}"
                                }
                            }
                        }"""
                        
                        sh "echo '$dockerConfig' > ~/.docker/config.json"
                        sh "docker push $DOCKER_IMAGE_NAME"


                        
                        
                    }

                    // Push the Docker image to DockerHub
                   // sh "docker push $DOCKER_IMAGE_NAME"
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    // Start Minikube (if not already running)
                    sh 'minikube start'

                    // Set Minikube context
                    sh 'kubectl config use-context minikube'

                    // Deploy the application using Kubernetes manifests
                    sh 'kubectl apply -f your-path-to-kubernetes-manifests'
                }
            }
        }
    }

    post {
        always {
            // Clean up: Stop Minikube
            script {
                sh 'minikube stop'
            }
        }
    }
}
