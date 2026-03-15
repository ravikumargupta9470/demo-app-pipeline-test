pipeline {
    agent any

    environment {
        DOCKER_HUB_CRED = credentials('dockerhub-cred')
        IMAGE_NAME = "ravikumargupta9470/demo-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ravikumargupta9470/demo-app-pipeline-test.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t $IMAGE_NAME:latest ."
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f deployment.yaml"
            }
        }
    }
}
