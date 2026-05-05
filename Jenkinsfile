pipeline {
    agent any

    stages {

        stage('Build (Static)') {
            steps {
                echo 'No build needed for static project'
            }
        }

        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli'
                }
            }
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=frontend-app \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://3.231.24.74:9000 \
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-app .'
            }
        }
    }
}