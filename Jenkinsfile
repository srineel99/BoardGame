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
                    sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=boardgame -Dsonar.projectName=BoardGame -Dsonar.sources=. -Dsonar.host.url=http://65.2.141.242:9000"
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-cred') {
                        def image = docker.build("srineel99/boardgame:latest")
                        image.push()
                    }
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image srineel99/boardgame:latest || true'
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh 'kubectl apply -f deployment-service.yaml'
            }
        }
    }
}
