pipeline {
    agent {
        kubernetes {
            label 'kaniko-test'
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:v1.22.0-debug
    command: ["/busybox/cat"]
    tty: true
'''
        }
    }
    
    stages {
        stage('Test Kaniko Build') {
            steps {
                container('kaniko') {
                    sh '''
                        echo "🔨 Testing Kaniko build..."
                        /kaniko/executor --context=. --no-push --tar-path=image.tar
                        echo "✅ Kaniko build successful"
                        ls -la image.tar
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo "🎉 SANITY CHECK PASSED: Kaniko works"
        }
    }
}