pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:14'
                    reuseNode true
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

                if [ ! -f package-lock.json ]; then
                    npm install --no-audit --no-fund
                fi

                npm install
                npm ci
                npm run build
                ls -la
                '''
            }
        }

    stage('Test') {
            agent {
                docker {
                    image 'node:14'
                    reuseNode true
                }
            }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
}

post {
        always {
            junit 'test-results/junit.xml'  // Fixing the typo in the path
        }
    }
}