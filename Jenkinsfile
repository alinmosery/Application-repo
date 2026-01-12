pipeline {
    agent any
    environment {
        // משיכת המפתחות מה-Credentials שהגדרת תחת ה-ID 'aws-creds'
        AWS_ACCESS_KEY_ID     = credentials('aws-creds')
        AWS_SECRET_ACCESS_KEY = credentials('aws-creds')
        
        ECR_REGISTRY   = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG      = "build-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Build & Push to ECR') {
            steps {
                script {
                    echo "Building Image..."
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                    
                    echo "Logging into Amazon ECR..."
                    // שימוש במשתני הסביבה שהגדרנו למעלה
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    
                    echo "Pushing Image..."
                    sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                }
            }
        }
    }
}
