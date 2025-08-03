pipeline {
    agent any

    tools {
        maven = 'Maven_3.9.11'
    }
    
    environment {
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn -f be-fintrack-master/pom.xml clean verify sonar:sonar -DskipTests'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Deploy Local') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }
}
