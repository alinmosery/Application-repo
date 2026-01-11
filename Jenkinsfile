pipeline {
    agent any
    environment {
        AWS_CREDENTIALS_ID = 'aws-creds' // המזהה שהגדרת ב-Credentials
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "build-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Build & Push to ECR') {
            steps {
                script {
                    echo "Building Image..."
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."

                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                        echo "Logging into Amazon ECR..."
                        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                        
                        echo "Pushing Image..."
                        sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
