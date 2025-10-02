pipeline {
    agent any

    tools {
        maven 'Maven-3.8.4'
        jdk 'OpenJDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/nhhao211/LearnCICD.git',
                    credentialsId: 'nhhao211'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t shop-manager:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop shop-manager || true'
                sh 'docker rm shop-manager || true'
                sh 'docker run -d -p 8080:8080 --name shop-manager shop-manager:${BUILD_NUMBER}'
            }
        }
    }
}