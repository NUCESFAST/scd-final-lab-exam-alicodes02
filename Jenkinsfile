pipeline {
    agent any
    environment {
        DOCKER_HUB_USERNAME = 'alicodes02' 
        DOCKER_HUB_PASSWORD = 'dockerhub123*'  
        DOCKER_HUB_REPO = 'alicodes02'  
    }
    stages {
        stage(' 21I-1170 Checkout') {
            steps {
                // Checkout the code from GitHub
                git url: 'https://github.com/NUCESFAST/scd-final-lab-exam-alicodes02.git', branch: 'master'  // Replace with your GitHub repo URL and branch
            }
        }
        stage('21I-1170 Install Dependencies') {
            steps {
                script {
                    // Install dependencies for each service
                    dir('client') {
                        sh 'npm install'
                    }
                    dir('Classrooms') {
                        sh 'npm install'
                    }
                    dir('Auth') {
                        sh 'npm install'
                    }
                    dir('event-bus') {
                        sh 'npm install'
                    }
                    dir('Post') {
                        sh 'npm install'
                    }
                }
            }
        }
        stage('21I-1170 Build Docker Images') {
            steps {
                script {
                    // Build Docker images for each service
                    dir('client') {
                        sh "docker build -t ${DOCKER_HUB_REPO}/client:latest ."
                    }
                    dir('Auth') {
                        sh "docker build -t ${DOCKER_HUB_REPO}/auth:latest ."
                    }
                    dir('Classrooms') {
                        sh "docker build -t ${DOCKER_HUB_REPO}/classrooms:latest ."
                    }
                    dir('event-bus') {
                        sh "docker build -t ${DOCKER_HUB_REPO}/event-bus:latest ."
                    }
                    dir('Post') {
                        sh "docker build -t ${DOCKER_HUB_REPO}/post:latest ."
                    }
                    // Build Docker image for MongoDB
                    sh "docker build -t ${DOCKER_HUB_REPO}/mongodb:latest -f mongo.Dockerfile ."
                }
            }
        }
        stage('21I-1170 Push Docker Images') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh "echo ${DOCKER_HUB_PASSWORD} | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin"
                    
                    // Push Docker images to Docker Hub
                    sh "docker push ${DOCKER_HUB_REPO}/auth:latest"
                    sh "docker push ${DOCKER_HUB_REPO}/client:latest"
                    sh "docker push ${DOCKER_HUB_REPO}/classrooms:latest"
                    sh "docker push ${DOCKER_HUB_REPO}/event-bus:latest"
                    sh "docker push ${DOCKER_HUB_REPO}/post:latest"
                    sh "docker push ${DOCKER_HUB_REPO}/mongodb:latest"
                }
            }
        }
        stage('21I-1170 Deploy and Validate') {
            steps {
                script {
                    // Deploy containers using docker-compose
                    sh 'docker-compose up -d'
                    
                    // Add necessary validation steps here
                    // Example: Check if the frontend is accessible
                    sh 'sleep 30'  // Wait for services to start
                    sh 'curl -f http://localhost:1170'  // Adjust the URL if necessary
                }
            }
        }
    }
    post {
        always {
            script {
                // Clean up: Shut down the containers
                sh 'docker-compose down'
            }
        }
    }
}
