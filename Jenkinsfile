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
            environment {
                HOME = "${WORKSPACE}"
                NPM_CONFIG_CACHE = "${HOME}/.npm"
            }
            steps {
                echo 'Building...'
                sh '''
                    echo "HOME is $HOME"
                    echo "NPM_CONFIG_CACHE is $NPM_CONFIG_CACHE"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
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
            environment {
                HOME = "${WORKSPACE}"
                NPM_CONFIG_CACHE = "${HOME}/.npm"
            }
            steps {
                echo 'Testing...'
                sh '''
                    echo "HOME is $HOME"
                    echo "NPM_CONFIG_CACHE is $NPM_CONFIG_CACHE"
                    test -f build/index.html
                    npm test
                '''
                junit 'test-results/junit.xml'
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                HOME = "${WORKSPACE}"
                NPM_CONFIG_CACHE = "${HOME}/.npm"
            }
            steps {
                echo 'Deploying...'
                sh '''
                    echo "HOME is $HOME"
                    echo "NPM_CONFIG_CACHE is $NPM_CONFIG_CACHE"
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    # Add your actual deploy command here
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
