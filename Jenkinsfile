pipeline {
    agent any
    tools {
        maven 'Maven-3.9.11' // Sử dụng Maven đã cài thủ công
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
                bat 'if exist "be-fintrack-master\\target" rmdir /s /q "be-fintrack-master\\target"'
                bat 'del /s /q "be-fintrack-master\\.mvn\\watcher.log" 2>nul'
                bat 'mvn -f be-fintrack-master/pom.xml clean --no-snapshot-updates --fail-never'
            }
        }
        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn -f be-fintrack-master/pom.xml clean install -DskipTests -Dmaven.wagon.http.ssl.insecure=true -Dmaven.central.mirror=https://maven.aliyun.com/repository/public -DrepositoryUrl=https://maven.aliyun.com/repository/public'
                }
            }
        }
        stage('Docker Build') {
            steps {
                bat 'docker-compose -f be-fintrack-master/docker-compose.yml build'
            }
        }
        stage('Deploy Local') {
            steps {
                bat 'docker-compose -f be-fintrack-master/docker-compose.yml down'
                bat 'docker-compose -f be-fintrack-master/docker-compose.yml up -d'
            }
        }
    }
}
