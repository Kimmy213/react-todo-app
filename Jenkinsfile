pipeline {
    agent none

    stages {
        stage('Checkout') {
            agent any
            steps {
                deleteDir()
                git branch: 'main',
                    url: 'https://github.com/Kimmy213/react-todo-app.git'
            }
        }

        stage('Build & Test') {
            agent {
                docker {
                    image 'node:18'
                }
            }
            steps {
                sh 'PUPPETEER_SKIP_DOWNLOAD=true npm install'
                sh 'npm run test'
            }
        }
    }
}
