pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '8f3a3fe6-5d9e-4bc1-bc38-437538c340dc'
        NETLIFY_AUTH_TOKEN = credentials('netlify-test')
    }

    stages {
        stage('build') {
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

        stage('test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
               sh 'npm test'
                
            }
        }
        stage('deploy') {
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
                    echo "Deploying to production. SITE ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod

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
