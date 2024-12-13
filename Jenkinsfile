pipeline {
    agent any

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
            steps{
                sh'''
                    echo "hello from test"
                '''
            }
        }
    }
}