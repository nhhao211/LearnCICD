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
                    docker stop learncicd || exit 0
                    docker rm learncicd || exit 0
                    docker run -d -p 8080:8080 --name learncicd learncicd:%BUILD_NUMBER%
                '''
            }
        }
    }
}