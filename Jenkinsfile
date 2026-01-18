pipeline {
  agent any

  environment {
    S3_BUCKET = "jenkins-static-36d9d0d2"
    AWS_REGION = "us-east-1"
  }

  stages {

    stage('Build') {
      steps {
        sh '''
          npm install
          npm run build
        '''
      }
    }

    stage('Upload to S3') {
      steps {
        sh '''
          aws s3 sync build/ s3://$S3_BUCKET --delete
        '''
      }
    }

    stage('Invalidate CloudFront') {
      steps {
        sh '''
          aws cloudfront create-invalidation \
          --distribution-id E2J9QY64PLEQ15 \
          --paths "/*"
        '''
      }
    }
  }
}
