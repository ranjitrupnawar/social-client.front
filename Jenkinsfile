pipeline {
    agent any

    environment {
        BACKEND_APP_NAME = 'backend'
        BACKEND_SOURCE_CODE_URL = 'https://github.com/ranjitrupnawar/social-server.back.git'
        BACKEND_BRANCH_NAME = 'main'
        FRONTEND_APP_NAME = 'frontend'
        FRONTEND_SOURCE_CODE_URL = 'https://github.com/ranjitrupnawar/social-client.front.git'
        FRONTEND_BRANCH_NAME = 'main'
    }

    stages {
        // Backend Pipeline
        stage('Backend Checkout') {
            steps {
                echo "Checking out backend branch: ${env.BACKEND_BRANCH_NAME}"
                checkout([$class: 'GitSCM', 
                    userRemoteConfigs: [[url: "${env.BACKEND_SOURCE_CODE_URL}"]],
                    branches: [[name: "*/${env.BACKEND_BRANCH_NAME}"]]
                ])
            }
        }

        stage('Backend Build') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running backend build..."
                    npm install
                    '''
                }
            }
        }
        stage('Running backend tests') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running backend tests"
                    npm run test
                    '''
                }
            }
        }
       
        steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running backend build..."
                    npm run build
                    '''
                }
            }
        }

        stage('Backend Deploy') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running backend deployment..."
                    pm2 stop all || true 
                    sleep 10
                    pm2 start index.js --name ${BACKEND_APP_NAME}
                    pm2 save
                    sleep 30
                    '''
                }
            }
        }

        // Frontend Pipeline
        stage('Frontend Checkout') {
            steps {
                echo "Checking out frontend branch: ${env.FRONTEND_BRANCH_NAME}"
                checkout([$class: 'GitSCM', 
                    userRemoteConfigs: [[url: "${env.FRONTEND_SOURCE_CODE_URL}"]],
                    branches: [[name: "*/${env.FRONTEND_BRANCH_NAME}"]]
                ])
            }
        }

        stage('Frontend Build') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running frontend build..."
                    npm install
                    sleep 10
                    '''
                }
            }
        }
        stage('Running frontend tests') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running frontend tests"
                    npm run test
                    sleep 10
                    '''
                }
            }
        }
        stage('Frontend build') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running frontend build"
                    npm run build
                    sleep 10
                    '''
                }
            }
        }
        stage('Frontend Deploy') {
            steps {
                script {
                    sh '''
                    #!/bin/bash
                    echo "Running frontend deployment..."
                    sleep 10
                    npm start
                    '''
                }
            }
        }
    }
}
