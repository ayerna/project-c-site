pipeline {
  agent any

  environment {
    ECR_REPO = "public.ecr.aws/d0z6a9w2/project-c"
    REGION = "ap-south-1"
  }

  stages {

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t project-c .'
      }
    }

    stage('Tag Image') {
      steps {
        sh 'docker tag project-c:latest $ECR_REPO:latest'
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
          kubectl rollout restart deployment web-c -n project-c
          '''
          }
      }

  }
}
