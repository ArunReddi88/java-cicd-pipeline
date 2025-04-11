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
