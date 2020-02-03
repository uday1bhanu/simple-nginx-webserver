pipeline {
  agent {
    kubernetes {
      label 'simple-nginx-webserver'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  containers:
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
"""
}
   }
  stages {
    stage('Build') {
      steps {
        container('docker') {
          sh """
             docker build -t uday1bhanu/simple-nginx-webserver:$BUILD_NUMBER .
          """
        }
      }
    }
  }
}