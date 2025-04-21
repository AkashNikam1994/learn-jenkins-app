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
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Inside Build stage"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    #npm run build
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:22-alpine'
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
        stage('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.51.1-noble'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                           npm install serve
                           node_modules/.bin/serve -s build -p 3000 &
                           sleep 10
                           npx playwright test --reporter=html
                        '''
                    }
                }

    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}