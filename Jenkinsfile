pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin"
        IMAGE_NAME = "cicd-demo"
        CONTAINER_NAME = "cicd-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/abhinayanand7/cicd-staging-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Run Container') {
            steps {
                sh """
                docker rm -f ${CONTAINER_NAME} || true

                docker run -d \\
                --name ${CONTAINER_NAME} \\
                -p 5002:5000 \\
                ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Test App') {
            steps {
                sh "curl http://localhost:5002 || true"
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs!'
        }
    }
}
