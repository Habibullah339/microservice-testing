pipeline {
    agent any
    
    environment {
        GITHUB_CREDENTIAL_ID = '33339'
        DOCKER_IMAGE_NAME = 'habib339/sample-image'
        KUBERNETES_MANIFESTS_PATH = ''
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

        stage('Deploy to Minikube') {
            steps {
                script {
                    // Set KUBERNETES_MANIFESTS_PATH to the current working directory
                    KUBERNETES_MANIFESTS_PATH = pwd()

                    sh 'minikube start'
                    sh 'kubectl config use-context minikube'

                    // Apply only Kubernetes manifest files with .yaml extension
                    //sh "kubectl apply -f ${KUBERNETES_MANIFESTS_PATH}/*.yaml"
                    sh "kubectl apply -f ${KUBERNETES_MANIFESTS_PATH}/deployment.yaml"
                    sh "kubectl apply -f ${KUBERNETES_MANIFESTS_PATH}/service.yaml"
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
