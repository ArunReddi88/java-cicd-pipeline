# Jenkinsfile
pipeline {
    agent any

    tools {
        maven 'Maven 3.8.4'
        jdk 'Java 11'
    }

    environment {
        SONARQUBE = credentials('sonarqube-token')
        DOCKER_REGISTRY = 'your-dockerhub-username/java-app'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/your-repo/java-app.git'
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
