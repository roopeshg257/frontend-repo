pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'rrrbk'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/roopeshg257/frontend-repo.git',
                    credentialsId: 'e5303cbf-9b0a-4310-a8b2-6d00824c1d91'
            }
        }

        stage('Build React') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withAWS(credentials: 'e5303cbf-9b0a-4310-a8b2-6d00824c1d91', region: 'us-east-1') {
                    sh 'aws s3 sync build/ s3://$S3_BUCKET --delete'
                    sh 'aws cloudfront create-invalidation --distribution-id E28B08W45JIKSL --paths "/*"'
                }
            }
        }
    }
}
