pipeline {
    agent any

    environment {
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
                sh "/usr/local/bin/docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Run Container') {
            steps {
                sh """
                /usr/local/bin/docker rm -f ${CONTAINER_NAME} || true

                /usr/local/bin/docker run -d \\
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
            echo '✅ CI/CD Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs!'
        }
    }
}
