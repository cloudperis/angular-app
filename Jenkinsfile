pipeline {
    agent any
    tools {
	    nodejs '14.21.2'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/cloudperis/angular-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install @angular/cli'
                sh 'npm install'
                sh 'ng build'
            }
        }
        stage('Deploy') {
            environment {
                S3_BUCKET_NAME = 'hugo-cicd-website'
            }
            steps {
                withAWS(region: 'us-east-1', credentials: 'hugo-aws-keys') {
                    s3Upload(pathStyleAccessEnabled: true, bucket: S3_BUCKET_NAME, workingDir: './dist/angular-app', includePathPattern: '**/*', excludePathPattern: '')
                }
            }
        }
    }
}
