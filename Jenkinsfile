pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {

        stage('Install & Test') {
            steps {
                sh '''
                docker run --rm \
                  -v $PWD:/app \
                  -w /app \
                  node:18 \
                  sh -c "npm install && npm run test"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t kimmy2/todo-app:latest .'
            }
        }

        stage('Login Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push kimmy2/todo-app:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop todo-container || true
                docker rm todo-container || true
                docker run -d -p 3000:3000 --name todo-container kimmy2/todo-app:latest
                '''
            }
        }

    }
}
