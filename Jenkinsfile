pipeline {
    agent any
    environment {
        IMAGE_NAME = "hello-docker"
        NEXUS_PORT = "5000"
        NEXUS_REPO = "hello-repo"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/a46002485-cyber/hello-docker.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Push to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh "echo $NEXUS_PASS | docker login -u $NEXUS_USER --password-stdin http://localhost:${NEXUS_PORT}"
                    sh "docker tag ${IMAGE_NAME} localhost:${NEXUS_PORT}/${NEXUS_REPO}/${IMAGE_NAME}:latest"
                    sh "docker push localhost:${NEXUS_PORT}/${NEXUS_REPO}/${IMAGE_NAME}:latest"
                }
            }
        }
    }
    post {
        failure {
            echo 'Build failed. Check logs!'
        }
    }
}
