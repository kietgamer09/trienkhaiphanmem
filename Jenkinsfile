pipeline {
    agent any
    tools {
        maven 'Maven-3.9.11' // Đảm bảo tên khớp với Global Tool Configuration
    }
    environment {
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kietgamer09/trienkhaiphanmem.git', credentialsId: '8d2fff40-4c3e-406c-bcfd-5232f3b6b4b8'
            }
        }
        stage('Reset Build Status') {
            steps {
                bat 'mvn -f be-fintrack-master/pom.xml validate --no-snapshot-updates' // Sửa lệnh
            }
        }
        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn -f be-fintrack-master/pom.xml clean verify sonar:sonar -DskipTests -Dmaven.wagon.http.ssl.insecure=true -Dmaven.repo.remote=https://maven.aliyun.com/repository/public'
                }
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker-compose build'
            }
        }
        stage('Deploy Local') {
            steps {
                bat 'docker-compose down'
                bat 'docker-compose up -d'
            }
        }
    }
}
