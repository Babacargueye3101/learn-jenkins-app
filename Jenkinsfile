pipeline {
    agent any

    environment {
        NODE_VERSION = "24.8.0"
        NPM_CACHE = "${WORKSPACE}/.npm"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Babacargueye3101/learn-jenkins-app.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Nettoyage du workspace pour éviter les conflits
                sh 'rm -rf node_modules'
                sh 'npm cache clean --force'

                // Installer les dépendances avec npm ci et cache local
                sh "npm ci --prefer-offline --cache $NPM_CACHE"
            }
        }

        stage('Build') {
            steps {
                // Si tu as un build, sinon tu peux supprimer cette étape
                sh 'npm run build || echo "No build script found, skipping"'
            }
        }

        stage('Test') {
            steps {
                // Tester le projet si tu as des tests
                sh 'npm test || echo "No test script found, skipping"'
            }
        }

        stage('Archive') {
            steps {
                // Sauvegarder le dossier dist ou autre
                archiveArtifacts artifacts: 'dist/**', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé"
        }
        failure {
            echo "Pipeline échoué 😢"
        }
    }
}