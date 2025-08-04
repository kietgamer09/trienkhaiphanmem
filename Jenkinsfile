pipeline {
    agent any

    tools {
        maven 'Maven-3.9.11'
    }

    environment {
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Docker Network & SonarQube Start') {
            steps {
                bat 'docker network create fintrack-net || echo network exists'

                bat 'docker-compose up -d sonarqube'

                bat 'ping 127.0.0.1 -n 60 >nul'
            }
        }

        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn -f be-fintrack-master/pom.xml clean verify sonar:sonar -DskipTests -Dsonar.token=squ_dcd86e3c448e482c0f0c5a42673e709aced3aa00'
                }
            }
        }

        stage('Quality Gate') {
            steps {
        timeout(time: 15, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
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
