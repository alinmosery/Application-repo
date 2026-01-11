pipeline {
    agent any

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        IMAGE_TAG = "pr-${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build & Push') {
            steps {
                script {
                    // הגדרת הכלי - ודאי שהשם כאן תואם למה שהגדרת ב-Tools
                    def dockerTool = tool name: 'my-docker', type: 'docker'
                    
                    // הוספת הנתיב של הכלי לסביבת הריצה
                    withEnv(["PATH+DOCKER=${dockerTool}/bin"]) {
                        echo "Docker tool found at: ${dockerTool}"
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
}
