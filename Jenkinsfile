pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "✅ Repository cloned"
            }
        }
        
        stage('Build Docker') {
            steps {
                sh '''
                    echo "🔨 Building Docker image..."
                    docker build -t python-test-app:$BUILD_NUMBER .
                    echo "✅ Build completed"
                    docker images | grep python-test-app
                '''
            }
        }
    }
    
    post {
        success {
            echo "🎉 SUCCESS: Docker image built!"
            echo "Image: python-test-app:${BUILD_NUMBER}"
        }
        failure {
            echo "❌ Build failed"
        }
    }
}