pipeline {
    agent any

    stages {
        stage('Clean Up Existing Containers') {
            steps {
                sh '''
                # Stop and remove all running containers
                docker stop $(docker ps -q) || true
                docker rm $(docker ps -a -q) || true
                '''
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/NikitaEeTu/learn-jenkins-app.git'
            }
        }

        stage('Build Docker Image and Run Tests') {
            steps {
                sh '''
                docker build -f /home/nikita/docker -t my-node-app:latest .
                '''
            }
        }

         stage('Run Tests') {
            steps {
                sh '''
                docker run --rm my-node-app:latest npm test
                '''
            }
        }

        stage('Run First Container on Port 3001') {
            steps {
                sh '''
                docker run -d --name node_app1 -p 3001:3000 my-node-app:latest
                '''
            }
        }

        stage('Run Second Container on Port 3002') {
            steps {
                sh '''
                docker run -d --name node_app2 -p 3002:3000 my-node-app:latest
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}