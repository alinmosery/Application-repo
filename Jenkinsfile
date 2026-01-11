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
            steps {
                script {
                    // וידוא שהפקודה קיימת ושג'נקינס יכול לגשת ל-Socket שפתחת
                    sh "docker --version"
                    
                    echo "Building Image: ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                    
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                        echo "Logging into ECR..."
                        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                        
                        echo "Pushing Image..."
                        sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
