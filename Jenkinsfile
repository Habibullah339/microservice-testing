pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'habib339'
        DOCKER_IMAGE_NAME = 'habib339/sample-microservice'
        GITHUB_TOKEN_CREDENTIALS_ID = '3338'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/habibullah339/microservice-testing.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    withCredentials([string(credentialsId: "${env.GITHUB_TOKEN_CREDENTIALS_ID}", variable: 'GITHUB_TOKEN')]) {
                        docker.withRegistry('https://registry.hub.docker.com', 'docker-credentials-id') {
                            def customImage = docker.build("${env.DOCKER_REGISTRY}/${env.DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
                            customImage.push()
                        }
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f kubernetes-deployment.yaml'
            }
        }
    }
}

