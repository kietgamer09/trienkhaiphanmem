pipeline {
    agent any
    tools {
        maven 'Maven-3.9.11' // Tên phải khớp với tên trong Global Tool Configuration
    }

    environment {
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn -f be-fintrack-master/pom.xml clean verify sonar:sonar -DskipTests -Dmaven.repo.remote=https://maven.aliyun.com/repository/public'
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
