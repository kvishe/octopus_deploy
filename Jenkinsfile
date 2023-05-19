pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        ECR_REGISTRY_URL = 'public.ecr.aws/w2f1s1x4/ecstest'
        IMAGE_TAG = 'latest'
        APP_NAME = 'Octo_Test'
        OCTOPUS_API_KEY = credentials('octopus-api-key')
        OCTOPUS_SERVER_URL = 'http://3.90.10.239:8080/'
        OCTOPUS_PROJECT_NAME = 'Test'
        OCTOPUS_ENVIRONMENT_NAME = 'dev'
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
                    docker.withRegistry('public.ecr.aws/w2f1s1x4/ecstest', 'ecr:us-east-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
    }
}
