pipeline {
    agent {
        docker {
            image 'gcr.io/kaniko-project/executor:debug'
            // ביטול ה-Entrypoint כדי שנוכל להריץ פקודות shell
            args '--entrypoint=""'
        }
    }

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "pr-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build & Push to ECR') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                    sh """
                    # יצירת קונפיגורציה של AWS עבור קניקו
                    mkdir -p /kaniko/.aws
                    echo -e "[default]\naws_access_key_id=${AWS_ACCESS_KEY_ID}\naws_secret_access_key=${AWS_SECRET_ACCESS_KEY}" > /kaniko/.aws/credentials
                    
                    # הרצת הבנייה והדחיפה ל-ECR בבת אחת
                    /kaniko/executor --dockerfile=Dockerfile \
                      --context=\$(pwd) \
                      --destination=${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}
                    """
                }
            }
        }
    }
}
