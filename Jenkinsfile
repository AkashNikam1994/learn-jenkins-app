pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Build') {
            agent {
                docker {
                    image: 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
             sh '''
                echo "Inside Build stage"
                ls -la
                node --version
                npm -- version
                npm ci
                npm run build
              '''
            }
        }
    }
}
