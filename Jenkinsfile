pipeline {
    agent any
    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
    }
    stages {
        stage('Build & Push') {
            steps {
                script {
                    def dockerImage = "${ECR_REGISTRY}/${ECR_REPOSITORY}:pr-${env.BUILD_NUMBER}"
                    
                    // פקודת בדיקה - לראות איפה הקובץ נמצא באמת
                    sh "find . -name Dockerfile"
                    
                    echo "Starting Build..."
                    // אם הקובץ בתיקייה הראשית - זה יעבוד. אם הוא ב-tests - שנה ל-tests/Dockerfile
                    sh "docker build -t ${dockerImage} -f \$(find . -name Dockerfile | head -n 1) ."
                    
                    docker.withRegistry("https://${ECR_REGISTRY}", "ecr:us-east-1:${AWS_CREDENTIALS_ID}") {
                        sh "docker push ${dockerImage}"
                    }
                }
            }
        }
    }
}
