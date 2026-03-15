pipeline {
    agent any

    tools {
        maven 'Maven-3.9'
    }

    environment {
        IMAGE_NAME = "ravikumargupta9470/demo-app"
        DOCKERHUB_CREDENTIALS = "dockerhub-cred"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ravikumargupta9470/demo-app-pipeline-test.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Docker Push') {
            steps {
                bat 'docker push %IMAGE_NAME%:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/deployment.yml'
                bat 'kubectl apply -f k8s/service.yml'
            }
        }
    }
}
