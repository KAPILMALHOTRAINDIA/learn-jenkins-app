pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build

                    ls -la
                '''
            }
        }
        stage("Test") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u root'
                }
            }
            steps {
                sh '''
                    npm test
                '''

                sh '''
                    if [ -f build/index.html ]; then
                        echo "index.html exists."
                    else
                        echo "index.html does not exist."
                        exit 1
                    fi
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.40.0-focal'
                    reuseNode true
                    args '-u root'
                }
            }

            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build -L 3001 &
                    sleep 10
                    npx playwright test
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