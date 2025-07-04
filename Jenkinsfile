pipeline {
    agent any

    environment {
        IMAGE_NAME = "rayansl/spring-boot-demo"
        CONTAINER_NAME = "spring-boot-container"
        KUBE_DEPLOY_FILE = "k8s/deployment.yaml"
        KUBE_SERVICE_FILE = "k8s/service.yaml"
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo '✅ Cloning repo automatically from GitHub...'
                // Git step handled by Jenkins SCM
            }
        }

        stage('Build JAR') {
            steps {
                echo '🔧 Giving gradlew execute permission...'
                sh 'chmod +x ./gradlew'

                echo '🔧 Building .jar file...'
                sh './gradlew clean build'
            }
        }

        stage('Login to DockerHub') {
            steps {
                echo '🔐 Logging into DockerHub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo '📤 Pushing image to DockerHub...'
                sh "docker push $IMAGE_NAME"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '🚀 Applying K8s Deployment...'
                sh "kubectl apply -f $KUBE_DEPLOY_FILE"
            }
        }

        stage('Expose Service') {
            steps {
                echo '🌐 Applying K8s Service...'
                sh "kubectl apply -f $KUBE_SERVICE_FILE"
            }
        }
    }

    post {
        success {
            echo '✅ All stages passed. App deployed on Kubernetes!'
        }
        failure {
            echo '❌ Build failed. Check the logs.'
        }
    }
}
