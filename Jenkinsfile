pipeline {
    agent any
    tools {
        maven 'Maven-3.9.11' // Đảm bảo tên khớp với Global Tool Configuration
    }
    environment {
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Reset Build Status') {
            steps {
                bat 'mvn --no-snapshot-updates'
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
