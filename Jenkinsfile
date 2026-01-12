pipeline {
    agent any
    environment {
        AWS_ID = credentials('aws-creds')
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "build-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Build & Push to ECR') {
            steps {
                script {
                    echo "Building Image..."
                    // שלב הבנייה שראינו שעובד אצלך
                    sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                    
                    withCredentials([usernamePassword(credentialsId: 'aws-creds', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                        echo "Installing AWS CLI and Logging in..."
                        // הפקודה הזו מתקינה את ה-CLI זמנית ומבצעת לוגין
                        sh """
                        apt-get update && apt-get install -u curl unzip -y
                        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
                        unzip -o awscliv2.zip
                        ./aws/install --update
                        /usr/local/bin/aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}
                        """
                        
                        echo "Pushing Image..."
                        sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
