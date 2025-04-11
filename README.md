# java-cicd-pipeline

├── Jenkinsfile
├── Dockerfile
├── README.md
├── k8s
│   ├── deployment.yaml
│   └── service.yaml
└── src
    └── main
        └── java
            └── App.java

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

# Dockerfile
FROM openjdk:11
COPY target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]

# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: java-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: java-app
  template:
    metadata:
      labels:
        app: java-app
    spec:
      containers:
      - name: java-app
        image: your-dockerhub-username/java-app:latest
        ports:
        - containerPort: 8080

# k8s/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: java-app-service
spec:
  type: LoadBalancer
  selector:
    app: java-app
  ports:
    - port: 80
      targetPort: 8080

# README.md
# Java CI/CD Pipeline with Jenkins, Docker, and Kubernetes

## Tools Used
- Java 11
- Maven
- Jenkins
- SonarQube
- Docker
- Kubernetes (Minikube or EKS)
- AWS EC2 (for Jenkins and SonarQube hosting)

## Pipeline Stages
1. Clone Java app from GitHub
2. Build the application using Maven
3. Run SonarQube for static code analysis
4. Build Docker image and push to DockerHub
5. Deploy the container to Kubernetes cluster

## Setup
1. Clone the repository
2. Install Jenkins, Docker, and configure Kubernetes access on Jenkins host
3. Configure Jenkins credentials for SonarQube and DockerHub
4. Run the pipeline

## Project Structure
- `Jenkinsfile`: Pipeline definition
- `Dockerfile`: Docker build instructions
- `k8s/`: Kubernetes manifests
- `src/`: Java application code

# src/main/java/App.java
public class App {
    public static void main(String[] args) {
        System.out.println("Hello from Java CI/CD Pipeline!");
    }
}
