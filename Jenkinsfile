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

                # If package-lock.json exists, use npm ci, otherwise run npm install
                if [ -f package-lock.json ]; then
                    npm ci --no-audit --no-fund
                else
                    npm install --no-audit --no-fund
                fi

                # Build the project
                npm run build

                # Show the directory contents
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
                # Check if the build output exists
                if [ -f build/index.html ]; then
                    echo "Build output found, running tests..."
                else
                    echo "Build output not found, build/index.html is missing!" && exit 1
                fi

                # Run tests
                npm test
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
