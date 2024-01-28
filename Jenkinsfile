pipeline {
    agent any
    
    environment {
        GITHUB_CREDENTIAL_ID = '33339'
        DOCKERHUB_CREDENTIAL_ID = '3338'  // Update with the actual credential ID
        DOCKER_IMAGE_NAME = 'habib339/sample-repo'  // Update with your desired repository name
        DOCKER_TAG = 'latest'  // Update with your desired tag
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
                script {
                    withCredentials([usernamePassword(credentialsId: '3338', usernameVariable: 'DOCKERHUB_CREDENTIALS_USR', passwordVariable: 'DOCKERHUB_CREDENTIALS_PSW')]) {
                        sh "echo \${DOCKERHUB_CREDENTIALS_PSW} | sudo docker login -u \${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                        echo 'Login Completed'
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh "docker tag $DOCKER_IMAGE_NAME ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                    sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
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
