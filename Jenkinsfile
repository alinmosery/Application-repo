script {
    // 1. התחברות ל-ECR (משתמש ב-Credentials של AWS שהגדרת בג'נקינס)
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
        
        echo "Logging into Amazon ECR..."
        sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
        
        // 2. בניית האימג' (חובה לתייג אותו עם כתובת ה-Registry)
        echo "Building image..."
        sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
        
        // 3. דחיפה ל-ECR
        echo "Pushing image..."
        sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
    }
}
