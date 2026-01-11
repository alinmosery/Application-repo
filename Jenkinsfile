pipeline {
    agent any

    environment {
        AWS_CREDENTIALS_ID = 'aws-creds'
        ECR_REGISTRY = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPOSITORY = 'alin-calculator'
        // הוספת הנתיב שבו נמצאת פקודת הדוקר שהעתקנו
        PATH = "/usr/bin:$PATH"
    }

    stages {
        stage('Build & Push') {
            when {
                changeRequest() 
            }
            steps {
                script {
                    def dockerImage = "${ECR_REGISTRY}/${ECR_REPOSITORY}:pr-${env.BUILD_NUMBER}"
                    
                    echo "Building image: ${dockerImage}"
                    
                    // שימוש בבלוק המובנה של ג'נקינס ל-ECR
                    docker.withRegistry("https://${ECR_REGISTRY}", "ecr:us-east-1:${AWS_CREDENTIALS_ID}") {
                        // בנייה של האימג' מה-Dockerfile שבשורש הפרויקט
                        def myApp = docker.build("${dockerImage}", ".")
                        
                        // דחיפה ל-ECR
                        myApp.push()
                    }
                }
            }
        }
    }
}
