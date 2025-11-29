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
                   echo "--- BUILD STAGE ---"
                   node --version
                   npm --version

                   npm ci
                   npm run build

                   echo "Build completed:"
                   ls -la build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                  echo "--- UNIT TEST STAGE ---"

                  test -f build/index.html   # vérifie que build existe
                  npm test --if-present
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.42.1-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                  echo "--- E2E TEST STAGE ---"

                  # Important : Playwright container est vide → il faut réinstaller les deps
                  npm ci

                  # Installer serve globalement
                  npm install -g serve

                  # Lancer le serveur React en arrière-plan
                  npx serve -s build -l 3000 &

                  # Laisser le serveur démarrer
                  sleep 5

                  # Lancer les tests Playwright
                  npx playwright test
                '''
            }
        }
    }
}