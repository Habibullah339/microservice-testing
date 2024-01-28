pipeline {
    agent any
    
    environment {
        GITHUB_CREDENTIAL_ID = '33339'
        DOCKERHUB_CREDENTIAL_ID = '3338'  // Update with the actual credential ID
        DOCKER_IMAGE_NAME = 'habib339/sample-image'
        DOCKERHUB_CREDENTIALS_USR = credentials('your-dockerhub-credential-id').username
        DOCKERHUB_CREDENTIALS_PSW = credentials('your-dockerhub-credential-id').password
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

        stage('Login to Docker Hub') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                echo 'Login Completed'
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh "docker tag $DOCKER_IMAGE_NAME ${DOCKER_IMAGE_NAME}:latest"
                    sh "docker push ${DOCKER_IMAGE_NAME}:latest"
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
