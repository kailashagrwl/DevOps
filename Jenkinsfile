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
                # Remove old test container if exists
                docker stop test-container || true
                docker rm test-container || true

                # Run new container
                docker run -d -p 5000:80 --name test-container my-app

                # Wait for app to start
                sleep 5

                # Health check
                curl -f http://localhost:5000 || exit 1

                # Cleanup
                docker stop test-container
                docker rm test-container
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                # Remove old production container
                docker stop my-container || true
                docker rm my-container || true

                # Run new container
                docker run -d -p 80:80 --name my-container my-app
                '''
            }
        }
    }
}