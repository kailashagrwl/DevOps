pipeline {
    agent any

    stages {

        stage('Build (Static)') {
            steps {
                echo 'No build needed for static project'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''
                    npx sonar-scanner \
                    -Dsonar.projectKey=frontend-app \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://3.231.24.74:9000 \
                    -Dsonar.login=$SONAR_AUTH_TOKEN
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