pipeline {
    agent any

    stages {
        stage('Run First Container on Port 3001') {
            agent {
                docker {
                    image 'node:14'
                    reuseNode true
                    args '-p 3001:3000'
                }
            }
            steps {
                sh '''
                npm --version
                node --version

                export NPM_CONFIG_CACHE=/tmp/.npm-cache
                export NPM_CONFIG_PREFIX=/tmp/.npm-global
                export NPM_CONFIG_USERCONFIG=/tmp/.npmrc
                export PATH=$PATH:/tmp/.npm-global/bin

                # Run the application on port 3000 inside the container
                npm start -- --port 3000
                '''
            }
        }

        stage('Run Second Container on Port 3002') {
            agent {
                docker {
                    image 'node:14'
                    reuseNode true
                    args '-p 3002:3000'
                }
            }
            steps {
                sh '''
                npm --version
                node --version

                export NPM_CONFIG_CACHE=/tmp/.npm-cache
                export NPM_CONFIG_PREFIX=/tmp/.npm-global
                export NPM_CONFIG_USERCONFIG=/tmp/.npmrc
                export PATH=$PATH:/tmp/.npm-global/bin

                # Run the application on port 3000 inside the container (but exposed as 3002)
                npm start -- --port 3000
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
