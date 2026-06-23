pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    mkdir .npm
                    sudo chown -R 119:124 "/.npm"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build

                    ls -la
                '''
            }
        }
    }
}