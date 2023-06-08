pipeline {
    agent any
    parameters {
        string(name: 'ECR_REPO_URL', defaultValue: "https://215598677274.dkr.ecr.us-east-1.amazonaws.com", description: 'URL of ECR repo')
        string(name: 'IMAGE_NAME', defaultValue: "underwater", description: 'Name of the Image')
        string(name: 'ENV', defaultValue: "Development", description: 'Octopus Environment')
        string(name: 'PROJECT', defaultValue: "Test", description: 'Octopus Project Name')
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
                    app = docker.build("${IMAGE_NAME}")
                    }
                }
            }
        
        stage('Push Docker Image to ECR') {
            steps {
                script{
                    docker.withRegistry( "${ECR_REPO_URL}", 'ecr:us-east-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
        stage('Deploy through Octopus'){
            steps {
                script{
                    octopusCreateRelease additionalArgs: '', cancelOnTimeout: false, channel: '', defaultPackageVersion: '', deployThisRelease: false, deploymentTimeout: '', environment: "${ENV}", jenkinsUrlLinkback: false, project: "${PROJECT}", releaseNotes: false, releaseNotesFile: '', releaseVersion: "1.0.${BUILD_NUMBER}", tenant: '', tenantTag: '', toolId: 'Default', verboseLogging: false, waitForDeployment: false
                    octopusDeployRelease cancelOnTimeout: false, deploymentTimeout: '', environment: "${ENV}", project: "${PROJECT}", releaseVersion: "1.0.${BUILD_NUMBER}", tenant: '', tenantTag: '', toolId: 'Default', variables: '', verboseLogging: false, waitForDeployment: true
                }
            }
        }
    }
}
