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
                bat 'docker build -t learnCICD:%BUILD_NUMBER% .'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                    docker stop shop-manager || exit 0
                    docker rm shop-manager || exit 0
                    docker run -d -p 8080:8080 --name shop-manager shop-manager:%BUILD_NUMBER%
                '''
            }
        }
    }
}