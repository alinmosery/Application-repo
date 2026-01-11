pipeline {
    agent any

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
    }

    stages {
        stage('Build & Push') {
            when {
                changeRequest() 
            }
            steps {
                script {
                    // הגדרת שם האימג' עם מספר ה-Build הנוכחי
                    def dockerImage = "${ECR_REGISTRY}/${ECR_REPOSITORY}:pr-${env.BUILD_NUMBER}"
                    
                    echo "Building image: ${dockerImage}"
                    
                    // שימוש בבלוק docker.withRegistry כדי לטפל בלוגין ובבנייה
                    docker.withRegistry("https://${ECR_REGISTRY}", "ecr:us-east-1:${AWS_CREDENTIALS_ID}") {
                        // בנייה של האימג'
                        def myApp = docker.build("${dockerImage}")
                        
                        // העלאה (Push) ל-ECR
                        myApp.push()
                    }
                }
            }
        }
    }
}
