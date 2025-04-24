pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/srineel99/BoardGame.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Quality Check') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=boardgame -Dsonar.projectName=BoardGame -Dsonar.sources=. -Dsonar.host.url=http://3.111.169.150:9000"
                }
            }
        }
    }
}
