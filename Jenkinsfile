pipeline {
    agent any
    parameters {
        string(name: "ECR_REPO_URL", description: "URL of ECR repo")
        string(name: "IMAGE_NAME", description: "Name of the Image")
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("underwater")
                    }
                }
            }
        
        stage('Deploy') {
            steps {
                script{
                    docker.withRegistry('$params.ECR_REPO_URL', 'ecr:us-east-2:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
