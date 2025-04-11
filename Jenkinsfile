pipeline {
    agent any

    tools {
        maven 'Maven3.8.7'
        jdk 'JAVA-17'
    }
     stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ArunReddi88/java-cicd-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Quality Check') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build("java-app:latest")
                          .push("${env.DOCKER_REGISTRY}")
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
