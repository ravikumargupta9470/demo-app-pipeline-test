pipeline {
    agent any

    tools {
        maven 'Maven-3.9'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/ravikumargupta9470/demo-app-pipeline-test.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t ravikumargupta9470/demo-app .'
            }
        }

        stage('Docker Push') {
            steps {
                bat 'docker push ravikumargupta9470/demo-app'
            }
        }
    }
}
