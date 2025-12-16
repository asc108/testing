pipeline {
    agent {
        kubernetes {
            label 'k8s-test-${UUID.randomUUID().toString()}'
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: test
    image: alpine:latest
    command: ["cat"]
    tty: true
'''
        }
    }
    
    stages {
        stage('Test K8s Connection') {
            steps {
                container('test') {
                    sh '''
                        echo "=== Test 1: Can container run commands? ==="
                        hostname
                        echo "Container works ✓"
                        
                        echo "=== Test 2: Can access Jenkins workspace? ==="
                        ls -la
                        echo "Workspace accessible ✓"
                        
                        echo "=== Test 3: Basic network test ==="
                        ping -c 2 google.com && echo "Network works ✓"
                    '''
                }
            }
        }
        
        stage('Test Docker/Kaniko') {
            steps {
                container('test') {
                    sh '''
                        echo "=== Test 4: Checking for Docker ==="
                        which docker || echo "Docker not found (expected for Kaniko)"
                        
                        echo "=== Test 5: Checking for Kaniko ==="
                        which kaniko || echo "Kaniko not in PATH (normal)"
                        
                        echo "=== Test 6: Creating test file ==="
                        echo "Test content" > test.txt
                        cat test.txt
                        echo "File operations work ✓"
                    '''
                }
            }
        }
        
        stage('Test Python') {
            steps {
                container('test') {
                    sh '''
                        echo "=== Test 7: Python test ==="
                        python --version 2>/dev/null || echo "Python not in this container"
                        echo "Using alpine, python not installed (normal)"
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo "🎉 ALL TESTS PASSED: Kubernetes agent is working!"
        }
        failure {
            echo "❌ Some tests failed - check Kubernetes/Jenkins configuration"
        }
    }
}