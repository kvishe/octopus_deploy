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
                    def dockerImage = docker.build('${APP_NAME}')
                    dockerImage.tag('${ECR_REGISTRY_URL}/${APP_NAME}:${IMAGE_TAG}')
                    dockerImage.push()
                    }
                }
            }
        
        stage('Deploy with Octopus') {
            steps {
                script {
                    sh "octo push --package=${APP_NAME}:${IMAGE_TAG} --replace-existing --server=${OCTOPUS_SERVER_URL} --apiKey=${OCTOPUS_API_KEY}"
                    sh "octo create-release --project=${OCTOPUS_PROJECT_NAME} --version=${IMAGE_TAG} --server=${OCTOPUS_SERVER_URL} --apiKey=${OCTOPUS_API_KEY}"
                    sh "octo deploy-release --project=${OCTOPUS_PROJECT_NAME} --deployTo=${OCTOPUS_ENVIRONMENT_NAME} --server=${OCTOPUS_SERVER_URL} --apiKey=${OCTOPUS_API_KEY}"
                }
            }
        }
    }
}
