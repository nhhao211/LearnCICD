pipeline {
    agent any

    tools {
        maven 'Maven-3.8.4'
        jdk 'OpenJDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/nhhao211/LearnCICD.git',
                    credentialsId: 'nhhao211'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t learncicd:%BUILD_NUMBER% .'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    echo "Stopping existing container..."
                    docker stop learncicd 2>nul || echo "No container to stop"
                    docker rm learncicd 2>nul || echo "No container to remove"

                    echo "Starting new container on port 8081..."
                    docker run -d -p 8081:8080 --name learncicd learncicd:%BUILD_NUMBER%

                    echo "Checking running containers..."
                    docker ps

                    echo "Waiting 10 seconds for app to start..."
                    timeout /t 10 /nobreak

                    echo "Checking container logs..."
                    docker logs learncicd
                '''
            }
        }
    }
}