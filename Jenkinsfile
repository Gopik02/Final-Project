pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'gopi02/node-hello-world'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    
    stages {
        // Stage 1: Checkout code
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Gopik02/Final-Project.git',
                        credentialsId: 'git-creds'
                    ]]
                ])
            }
        }
        
        // Stage 2: Build with Node.js
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        // Stage 3: Build Docker image
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        
        // Stage 4: Push to Docker Hub
        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh """
                            echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin
                            docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                        """
                    }
                }
            }
        }
    }
}