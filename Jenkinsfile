pipeline {
    agent any 
     
    tools {
        nodejs "node18"
    }
    environment {
        AWS_REGION = 'us-east-1'
        S3_BUCKET = 'static-website-bucket-29'
        BUILD_DIR = 'dist'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/kavyakr9599-dotcom/static-web.git'
            }
        }
         stage('Build') {
            steps {
                echo "installing dependencies"
            
                sh 'npm install'
                
                echo "building static website"
                 
                sh 'npm run build'
                 
               echo "archiving build artifacts"
               archiveArtifacts artifacts: 'dist/**'
            }
         }
          
         stage('deploy on s3') {
            steps {
                echo "Deploying application to s3"

                sh """
                aws s3 sync ${BUILD_DIR}/ s3://${S3_BUCKET} --delete
                """
            }
         }
    }
    post {
        success {
            echo "deployment completed successfully"
        }
        failure {
            echo "pipeline failed"
        }
    }
}