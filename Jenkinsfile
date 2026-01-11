pipeline {
    agent any

    environment {
        // הכתובת המדויקת של ה-ECR שלך
        ECR_IMAGE_URI = '992382545251.dkr.ecr.us-east-1.amazonaws.com/alin-calculator'
        // הכתובת הראשית של השרת (בשביל הלוגין)
        ECR_REGISTRY_ROOT = '992382545251.dkr.ecr.us-east-1.amazonaws.com'
        
        AWS_REGION = 'us-east-1'
        AWS_CREDS_ID = 'aws-creds' // השם של המפתחות שהגדרנו כרגע
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    echo 'Building Docker Image...'
                    // בונים את האימג'
                    sh "docker build -t $ECR_IMAGE_URI:pr-${BUILD_NUMBER} ."
                    
                    echo 'Running Tests...'
                    // מריצים טסטים זריזים
                    sh "docker run --rm $ECR_IMAGE_URI:pr-${BUILD_NUMBER} python3 -m unittest discover tests"
                }
            }
        }

        stage('Push to ECR (PR Only)') {
            // רץ רק אם זה Pull Request
            when {
                changeRequest() 
            }
            steps {
                script {
                    echo 'Publishing to ECR...'
                    
                    // התחברות ל-AWS עם המפתחות
                    withCredentials([usernamePassword(credentialsId: AWS_CREDS_ID, passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                        sh '''
                            # התחברות ל-Docker של AWS
                            aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REGISTRY_ROOT
                            
                            # העלאת האימג' שיצרנו
                            docker push $ECR_IMAGE_URI:pr-${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
    }
}