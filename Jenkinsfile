// filename: Jenkinsfile
pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'amzn-s3-jenkins-bucket'
        S3_PREFIX = 'my-app/main'
    }

    stages {
        stage('Verify AWS access') {
            steps {
                sh 'aws sts get-caller-identity'
            }
        }

        stage('Checkout and upload') {
            steps {
                checkout scm
                sh '''
                    aws s3 sync . "s3://${S3_BUCKET}/${S3_PREFIX}/" \
                      --exclude ".git/*" \
                      --exclude ".env" \
                      --exclude "**/.env"
                '''
            }
        }
    }

    post {
        success {
            echo 'Repository files uploaded to S3.'
        }
        failure {
            echo 'Pipeline failed. Review the console output.'
        }
    }
}
