pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        IMAGE_NAME = "adservice"
        ECR_REPO = "threetierapp"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Get AWS Account ID') {
            steps {
                script {
                    env.AWS_ACCOUNT_ID = sh(
                        script: "aws sts get-caller-identity --query Account --output text",
                        returnStdout: true
                    ).trim()

                    env.ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
                }
            }
        }

        stage('Login to Amazon ECR') {
            steps {
                sh """
                aws ecr get-login-password --region ${AWS_REGION} | \
                docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('src') {
                    sh """
                    docker build -t ${ECR_URI}/${IMAGE_NAME}:${IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                docker push ${ECR_URI}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
    }
}
