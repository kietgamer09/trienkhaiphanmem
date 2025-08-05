pipeline {
    agent any
    tools {
        maven 'Maven-3.9.11' // Sử dụng Maven đã cài thủ công
    }
    environment {
        IMAGE_TAG = "latest"
        SONAR_HOST_URL = "http://localhost:9000" // Thay bằng URL SonarQube của bạn
        SONAR_LOGIN = credentials('sonar-token') // ID token SonarQube trong Jenkins
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
                    // Thêm sonar.projectKey để xác định dự án trong SonarQube
                    bat 'mvn -f be-fintrack-master/pom.xml clean verify sonar:sonar -DskipTests -Dsonar.token=squ_dcd86e3c448e482c0f0c5a42673e709aced3aa00'
                }
            }
        }
         //stage('Quality Gate') {
            //steps {
        //timeout(time: 15, unit: 'MINUTES') {
            //waitForQualityGate abortPipeline: true
                //}
            //}
        //}
        stage('Docker Build') {
            steps {
                bat 'docker-compose build'
            }
        }
         stage('Deploy Local') {
            steps {
                bat 'docker-compose -f docker-compose.yml down'
                bat 'docker-compose -f docker-compose.yml up -d'
            }
        }
    }
}

