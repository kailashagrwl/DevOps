pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                docker run -d -p 5000:80 --name test-container my-app
                sleep 5
                curl -f http://localhost:5000 || exit 1
                docker stop test-container && docker rm test-container
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop my-container || true
                docker rm my-container || true
                docker run -d -p 80:80 --name my-container my-app
                '''
            }
        }
    }
}