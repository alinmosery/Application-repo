pipeline {
    agent any

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "pr-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build & Push') {
            when { changeRequest() }
            steps {
                script {
                    echo "Building Docker Image..."
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                    
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                        echo "Logging into Amazon ECR..."
                        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                        
                        echo "Pushing image to ECR..."
                        sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
