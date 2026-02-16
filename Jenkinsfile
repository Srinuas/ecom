pipeline {
    agent any
    environment {
        scannerHome = tool 'snr'
    }
    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t srinuas/onlineshop:paymentservice ."
                    }
                }
            }
        }
        stage('cqa') {
            steps {
                script {
                    withSonarQubeEnv('snr') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=myproj -Dsonar.projectName='myproj'"
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push srinuas/onlineshop:paymentservice"
                    }
                }
            }
        }
    }
}
