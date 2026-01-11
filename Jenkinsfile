pipeline {
    agent any

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "pr-${env.BUILD_NUMBER}"
        REGION = "us-east-1"
    }

    stages {
        stage('Build & Push to ECR') {
            when { changeRequest() }
            steps {
                // שימוש ב-Plugin המובנה של AWS ECR שחוסך פקודות Docker CLI
                script {
                    echo "Building and Publishing Image to ECR..."
                    // הפקודה הזו משתמשת ב-Plugin פנימי ולא ב-sh 'docker build'
                    ecrPayload = [
                        repositoryName: "${ECR_REPOSITORY}",
                        region: "${REGION}",
                        credentialsId: "${AWS_CREDENTIALS_ID}",
                        imageTag: "${IMAGE_TAG}"
                    ]
                    // אם הפלאגין מותקן, זה יעבוד. אם לא, נשתמש בשיטת ה-Raw
                    step([$class: 'ECRPublisher', 
                          region: "${REGION}", 
                          credentialsId: "${AWS_CREDENTIALS_ID}", 
                          imageName: "${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"])
                }
            }
        }
    }
}
