pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="449896976179"
        AWS_DEFAULT_REGION="us-east-1" 
        IMAGE_REPO_NAME="jenkins-docker-demo"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "449896976179.dkr.ecr.us-east-1.amazonaws.com/jenkins-docker-demo:latest"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 449896976179.dkr.ecr.us-east-1.amazonaws.com"
                }
                 
            }
        }
        
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/mydemo-feature']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/tcnaganath84/demo_CI_CD.git']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
         }
        }
      }
    }
}