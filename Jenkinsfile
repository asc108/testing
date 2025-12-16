pipeline {
    agent any
    stages {
        stage('Build Docker') {
            steps {
                sh 'docker build -t test-app:$BUILD_NUMBER .'
            }
        }
    }
}