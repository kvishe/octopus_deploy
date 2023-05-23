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
                 sh(label: 'ECR login and docker push', script:
                 '''
                  #!/bin/bash
                    echo "Authenticate with ECR"
                    docker login --username AWS --password $(aws ecr-public get-login-password --region us-east-1) public.ecr.aws/w2f1s1x4
          
                    docker tag underwater:latest public.ecr.aws/w2f1s1x4/ecstest:latest
                    docker push public.ecr.aws/w2f1s1x4/ecstest:latest
                 '''.stripIndent())
                }
            }
        }
    }
}
