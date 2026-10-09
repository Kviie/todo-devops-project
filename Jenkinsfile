
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Kviie/todo-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t todo-app:${BUILD_NUMBER} .'
            }
        }

        stage('Test Docker Image') {
            steps {
                sh 'docker image inspect todo-app:${BUILD_NUMBER}'
            }
        }

        stage('Pipeline Success') {
            steps {
                echo 'To-Do application Docker image built successfully!'
            }
        }
    }
}
