pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        AWS_ACCOUNT_ID = "666177270376"
        ECR_REPO = "threetierapp"
        IMAGE_TAG = "paymentservice-latest"
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
    }

    stages {

        stage('Login to Amazon ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} | \
                docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${ECR_URI}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push ${ECR_URI}:${IMAGE_TAG}
                '''
            }
        }
    }
}
