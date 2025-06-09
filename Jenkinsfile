pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // Replace with actual Jenkins credentials ID
        DOCKERHUB_USER = 'tpathak21'
    }

    stages {

        stage('Checkout Code from GitHub') {
            steps {
                git 'https://github.com/TejPATHAK/DevOps-CI-CD-Pipeline-using-Jenkins-Docker-and-Kubernetes.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                    echo "Building Backend Image"
                    docker build -t $DOCKERHUB_USER/devops-backend:latest ./backend

                    echo "Building Frontend Image"
                    docker build -t $DOCKERHUB_USER/devops-frontend:latest ./frontend
                '''
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_USER" --password-stdin
                '''
            }
        }

        stage('Push Docker Images') {
            steps {
                sh '''
                    docker push $DOCKERHUB_USER/devops-backend:latest
                    docker push $DOCKERHUB_USER/devops-frontend:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f k8s/deployment.yaml
                    kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
