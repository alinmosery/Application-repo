pipeline {
    agent any
    environment {
        ECR_REGISTRY   = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG      = "build-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Build & Push') {
            steps {
                script {
                    echo "Building Image..."
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                    
                    // שימוש בשיטה הישנה והטובה של ג'נקינס למשיכת ה-Keys
                    withCredentials([usernamePassword(credentialsId: 'aws-creds', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                        
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
