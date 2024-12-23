pipeline {
    agent any

    environment {
        CLOUD_CREDENTIALS_ID = 'cloud-credentials' // Replace with your Jenkins credentials ID
        DEPLOYMENT_BUCKET = 'your-bucket-name' // Replace with your cloud bucket name
        DEPLOYMENT_REGION = 'your-region' // Replace with your region
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from repository...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                // Add your build command, e.g., npm, mvn, or Gradle
                sh 'npm install && npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add your test command, e.g., npm test, mvn test
                sh 'npm test'
            }
        }

        stage('Deploy to Cloud') {
            steps {
                echo 'Deploying to cloud...'
                withCredentials([string(credentialsId: CLOUD_CREDENTIALS_ID, variable: 'CLOUD_AUTH')]) {
                    // Example using AWS CLI (adjust for your cloud provider)
                    sh '''
                    export AWS_ACCESS_KEY_ID=$(echo $CLOUD_AUTH | jq -r '.accessKeyId')
                    export AWS_SECRET_ACCESS_KEY=$(echo $CLOUD_AUTH | jq -r '.secretAccessKey')
                    aws s3 sync ./build s3://$DEPLOYMENT_BUCKET --region $DEPLOYMENT_REGION
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline succeeded! 🎉'
        }
        failure {
            echo 'Pipeline failed! ❌'
        }
    }
}
