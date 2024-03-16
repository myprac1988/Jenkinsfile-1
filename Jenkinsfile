pipeline {
    agent any
    environment {
            AWS_ACCOUNT_ID="339712930803"
            AWS_DEFAULT_REGIOS="ap-south-1"
            IMAGE_REPO_NAME="myprac"
            REPOSITORY_URL="public.ecr.aws/q3w8l9w9/myprac"
            DOCKER_TAG = getDockerTag()
    }
    
    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main',url: 'https://github.com/myprac1988/Dockerfile.git'
            }
        }
        stage('Login to ECR') {
            steps {
                sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/q3w8l9w9'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myprac:${DOCKER_TAG} .'
            }
        }
        stage('Docker tag image') {
            steps {
                sh 'docker tag myprac:${DOCKER_TAG} public.ecr.aws/q3w8l9w9/myprac:${DOCKER_TAG}'
            }
        }
        stage('Docker push image') {
            steps {
                sh 'docker push public.ecr.aws/q3w8l9w9/myprac:${DOCKER_TAG}'
            }
        }
    }
}
def getDockerTag(){
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
