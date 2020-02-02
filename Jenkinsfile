pipeline {
  agent { label 'docker' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  triggers {
    cron('@daily')
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