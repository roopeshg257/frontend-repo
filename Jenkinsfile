pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'rrrbk'
        CLOUDFRONT_DISTRIBUTION_ID = 'ENF3FM7WHDI3K'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/roopeshg257/frontend-repo.git'
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                  chmod +x build.sh
                  ./build.sh
                '''
            }
        }

        stage('Deploy to S3 & Invalidate CloudFront') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'awsid'
                ]]) {
                    sh '''
                      aws s3 sync build/ s3://$S3_BUCKET --delete
                      aws cloudfront create-invalidation \
                        --distribution-id $CLOUDFRONT_DISTRIBUTION_ID \
                        --paths "/*"
                    '''
                }
            }
        }
    }
}
