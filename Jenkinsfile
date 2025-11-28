pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-slim'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   ls -la
                   node --version
                   npm --version
                   npm ci --prefer-offline
                   npm run build
                '''
            }
        }
    }
}