pipeline {
    agent any

    tools {
        maven 'Default'
    }

    stages {
        stage('Checkout') {
            steps {
                bat 'echo Cloning done automatically by Jenkins'
            }
        }

        stage('Build') {
            steps {
                bat './mvnw clean package'
            }
        }

        stage('Test') {
            steps {
                bat './mvnw test'
            }
        }
    }
}
