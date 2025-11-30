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
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Unit Tests') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # IMPORTANT : générer junit.xml
                    CI=true npm test -- --watchAll=false

                    # Vérification (optionnelle)
                    ls -la jest-results || true
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    npx serve -s build -l 3000 &

                    # Attendre le démarrage du serveur
                    sleep 5

                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            echo "Collecting test results"
            junit 'jest-results/junit.xml'
        }
    }
}