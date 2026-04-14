pipeline {
  agent any
  environment {
    IMAGE_NAME = "cicd-staging-demo"
    CONTAINER_NAME = "staging-container"
  }
  stages {
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/abhinayanand7/cicd-staging-demo.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:latest")
        }
      }
    }
    stage('Stop Existing Container') {
      steps {
        sh "docker stop ${CONTAINER_NAME} || true"
        sh "docker rm ${CONTAINER_NAME} || true"
      }
    }
    stage('Deploy to Staging') {
      steps {
        sh """
          docker run -d \
          --name ${CONTAINER_NAME} \
          -p 5000:5000 \
          ${IMAGE_NAME}:latest
        """
      }
    }
    stage('Post-Deployment Verification') {
      steps {
        sh "curl http://localhost:5000 || true"
      }
    }
  }
}
