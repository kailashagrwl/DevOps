pipeline {
    agent {
        docker {
            image 'node:18'
        }
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm run build'
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