pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'  
        ECR_REPO = '221615956924.dkr.ecr.ap-south-1.amazonaws.com/tsp-fiu-api'
        IMAGE_TAG = 'latest'    
    }

    stages {
        
        stage('Authenticate with ECR') {
            steps {
                script {
                    // Login to ECR
                    sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO}"
                }
            }
        }
    
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it
                    sh "docker build -t ${ECR_REPO}:${IMAGE_TAG} ."

                    // Push the Docker image to ECR
                    sh "docker push ${ECR_REPO}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo "Image successfully pushed to ${ECR_REPO}:${IMAGE_TAG}"
        }
        failure {
            echo "Pipeline failed. Check the logs for details."
        }
    }
}
