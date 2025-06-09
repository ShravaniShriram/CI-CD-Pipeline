pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Jenkins DockerHub credentials ID
        DOCKERHUB_USERNAME = "tpathak21"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/TejPATHAK/DevOps-CI-CD-Pipeline-using-Jenkins-Docker-and-Kubernetes.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/devops-backend:latest", "./backend")
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    docker.build("${env.DOCKERHUB_USERNAME}/devops-frontend:latest", "./frontend")
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-creds') {
                        echo 'Logged in to DockerHub'
                    }
                }
            }
        }

        stage('Push Backend Image') {
            steps {
                script {
                    docker.image("${env.DOCKERHUB_USERNAME}/devops-backend:latest").push()
                }
            }
        }

        stage('Push Frontend Image') {
            steps {
                script {
                    docker.image("${env.DOCKERHUB_USERNAME}/devops-frontend:latest").push()
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/backend-deployment.yaml'
                sh 'kubectl apply -f k8s/frontend-deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
