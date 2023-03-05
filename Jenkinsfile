pipeline {
    agent any
    tools {
	    nodejs '14.21.2'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install @angular/cli'
                sh 'npm install'
                sh 'ng build'
            }
        }
        stage('Deploy') {
            environment {
                S3_BUCKET_NAME = 'anjola-cicd-website'
            }
            steps {
                withAWS(region: 'us-west-2', credentials: 'anjola-aws-keys') {
                    s3Upload(pathStyleAccessEnabled: true, bucket: S3_BUCKET_NAME, workingDir: './dist/angular-app', includePathPattern: '**/*', excludePathPattern: '')
                }
            }
        }
    }
}
