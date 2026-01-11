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
                    // פקודה שמורידה ומתקינה את דוקר לתוך התיקייה של ג'נקינס
                    def dockerBin = tool name: 'my-docker', type: 'docker'
                    
                    // הוספת הנתיב של דוקר ל-PATH של הריצה הנוכחית
                    withEnv(["PATH+DOCKER=${dockerBin}/bin"]) {
                        echo "Building Image using installed Docker tool..."
                        sh "docker build -t ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG} ."
                        
                        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: "${AWS_CREDENTIALS_ID}"]]) {
                            echo "Logging into ECR..."
                            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                            sh "docker push ${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}"
                        }
                    }
                }
            }
        }
    }
}
