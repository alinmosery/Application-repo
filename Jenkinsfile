pipeline {
    agent any
    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "pr-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Build & Push to ECR') {
            steps {
                script {
                    // שימוש ב-Credentials של AWS כדי להתחבר
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                        
                        echo "Building Image..."
                        // הפקודה מחפשת את ה-Dockerfile בשורש הפרויקט
                        sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                        
                        echo "Logging into Amazon ECR..."
                        // הפקודה שביקשת: שילוב של aws ecr login ו-docker login
                        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                        
                        echo "Pushing Image to ECR..."
                        sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
