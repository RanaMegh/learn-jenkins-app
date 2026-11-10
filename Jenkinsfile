pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '47658b67-55ca-43e0-8fd0-514b16f18812'
    }

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
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
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
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. SIte ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
        success {
            archiveArtifacts artifacts: 'build/**'
        }
    }
}