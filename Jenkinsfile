pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/a46002485-cyber/hello-docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t hello-docker .'
            }
        }

        stage('Push to Nexus') {
            steps {
                sh 'docker tag hello-docker localhost:8081/repository/khedr22_9/hello-docker:latest'
                sh 'docker push localhost:8081/repository/khedr22_9/hello-docker:latest'
            }
        }
    }
}
