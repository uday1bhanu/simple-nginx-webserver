pipeline {
    agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: docker
spec:
  containers:
  - name: docker
    image: docker:18.09.8-dind
    command:
    - dockerd-entrypoint.sh 
    - --storage-driver=overlay2
    tty: true
    securityContext:
    - privileged: true
"""
    }
  }
    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(daysToKeepStr: '365'))
        timeout(time: 90, unit: 'MINUTES')
    }
    stages {
    stage('Build') {
      steps {
        sh "docker build -t uday1bhanu/simple-nginx-webserver:${env.BUILD_NUMBER} ."
      }
    }
    stage('Publish') {
      when {
        branch 'master'
      }
      steps {
        withDockerRegistry([ credentialsId: "jenkins-docker", url: "hub.docker.com" ]) {
          sh 'docker push uday1bhanu/simple-nginx-webserver:${env.BUILD_NUMBER}'
        }
      }
    }
  }
}