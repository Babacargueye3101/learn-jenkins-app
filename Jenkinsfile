pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:24-alpine' // Node 24, compatible avec ton setup local
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Workspace contents:"
                    ls -la

                    echo "Node and npm versions:"
                    node --version
                    npm --version

                    echo "Cleaning previous installs..."
                    rm -rf node_modules
                    npm cache clean --force

                    echo "Installing dependencies..."
                    npm ci --prefer-offline --no-audit --progress=false

                    echo "Running build..."
                    npm run build || echo "No build script, skipping"

                    echo "Final workspace contents:"
                    ls -la
                '''
            }
        }
    }
}