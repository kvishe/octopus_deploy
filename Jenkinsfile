pipeline {
    agent any
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
                    docker.withRegistry('public.ecr.aws/w2f1s1x4/ecstest', 'ecr:us-east-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
