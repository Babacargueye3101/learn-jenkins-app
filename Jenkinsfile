pipeline {
    agent any

    stages {
        stage('Parallel Stages') {
            parallel {
                stage('Build') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "=== BUILD START ==="
                            node --version
                            npm --version
                            npm ci
                            npm run build
                            ls -la build
                            echo "=== BUILD END ==="
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
                            echo "=== TEST START ==="
                            npm ci
                            npm test
                            echo "=== TEST END ==="
                        '''
                    }
                }

                stage('Lint') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "=== LINT START ==="
                            npm ci
                            npm run lint || true
                            echo "=== LINT END ==="
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé"
            junit allowEmptyResults: true, testResults: '**/test-results/*.xml'
        }
    }
}