pipeline {
    agent any

    environment {
        IMAGE_NAME = 'todo-app'
        CONTAINER_NAME = 'todo-app'
        APP_PORT = '8082'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Kviie/todo-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'docker image inspect ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart unless-stopped \
                        -p ${APP_PORT}:80 \
                        ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    sleep 3
                    docker ps --filter "name=${CONTAINER_NAME}"
                    curl --fail --retry 5 --retry-delay 2 \
                        http://localhost:${APP_PORT}/
                '''
            }
        }
    }

    post {
        success {
            echo 'To-Do application deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
