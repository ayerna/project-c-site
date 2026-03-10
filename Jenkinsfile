pipeline {
  agent any

  environment {
    ECR_REPO = "709436855390.dkr.ecr.ap-south-1.amazonaws.com/project-a"
    REGION = "ap-south-1"
  }

  stages {

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t project-a .'
      }
    }

    stage('Tag Image') {
      steps {
        sh 'docker tag project-a:latest $ECR_REPO:latest'
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region $REGION \
        | docker login --username AWS \
        --password-stdin $ECR_REPO
        '''
      }
    }

    stage('Push Image') {
      steps {
        sh 'docker push $ECR_REPO:latest'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          kubectl apply -f deployment.yaml
          kubectl rollout restart deployment web-a -n project-a
          '''
          }
      }

  }
}
