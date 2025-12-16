pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:latest
"""
    }
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build image (no push)') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor \
              --context=. \
              --dockerfile=Dockerfile \
              --no-push
              --tar-path=/workspace/image.tar
          '''
        }
      }
    }
  }
}
