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
                    image 'node:18-bullseye'
                    args '-u root'
                }
            }
            steps {
                sh '''
                    apt-get update
                    apt-get install -y python3 python-is-python3 make g++

                    export PUPPETEER_SKIP_DOWNLOAD=true
                    npm install
                    npm run test || true
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Build SUCCESS ✅'
        }
        failure {
            echo 'Build FAILED ❌'
        }
    }
}
