pipeline {
    agent any

    // החלק הזה קריטי - הוא מחבר את הגדרת ה-Tool שעשית בג'נקינס לקוד
    tools {
        dockerTool 'my-docker'
    }

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "pr-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    echo "Building Docker Image..."
                    // שימוש בפקודת docker רגילה - ג'נקינס כבר ידע איפה היא בזכות ה-tools
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to ECR') {
            when {
                changeRequest()
            }
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                    script {
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
