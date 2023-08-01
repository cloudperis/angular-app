pipeline {
    agent any
    tools {
	nodejs '14.21.2'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install -g @angular/cli'
                sh 'npm install'
                sh 'ng build'
            }
        }
        stage('Deploy') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
                AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
                S3_BUCKET_NAME = 'hugo-cloudperis'
            }
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-credentials') {
                    s3Upload(pathStyleAccessEnabled: true, bucket: S3_BUCKET_NAME, workingDir: './dist/angular-app', includePathPattern: '**/*', excludePathPattern: '')
                }
            }
        }
    }
}
