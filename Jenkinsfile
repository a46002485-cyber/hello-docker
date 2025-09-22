pipeline {
    agent any

    environment {
        NEXUS_URL = "localhost:5050"
        IMAGE_NAME = "hello-docker"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/a46002485-cyber/hello-docker.git', credentialsId: '3b9c1404-85ad-4cea-98df-165818e43614']]
                ])
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
                    sh "docker login -u $NEXUS_USER -p $NEXUS_PASS $NEXUS_URL"
                    sh "docker tag ${IMAGE_NAME} $NEXUS_URL/${IMAGE_NAME}:latest"
                    sh "docker push $NEXUS_URL/${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Build and push completed successfully!'
        }
        failure {
            echo 'Build failed. Check logs!'
        }
    }
}
