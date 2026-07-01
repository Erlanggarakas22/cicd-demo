pipeline {
    agent any

    environment {
        IMAGE_NAME = "flask-demo"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "flask-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Erlanggarakas22/cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build \
                -t ${IMAGE_NAME}:${IMAGE_TAG} \
                -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Show Images') {
            steps {
                sh 'docker images | grep flask-demo'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f ${CONTAINER_NAME} || true'
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 5000:5000 \
                ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'sleep 5'
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo "Deployment Version ${IMAGE_TAG} Success"
        }

        failure {
            echo "Deployment Failed"
        }
    }
}
