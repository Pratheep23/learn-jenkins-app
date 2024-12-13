pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'e12ad433-992c-4dcb-9296-0ab370b06f02'
        NETLIFY_AUTH_TOKEN = credentials('NETLIFY_TOKEN')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            } //'npm ci' is basically 'npm install' but it has been specifically designed for cont integration servers
            steps{
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
            steps{
                sh'''
                    echo "hello from test"
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage('Deloy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh'''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }

    post {
        always{
            junit 'test-results/junit.xml'
        }
    }
}