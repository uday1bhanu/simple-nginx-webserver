def docker() {
    return [
        name: "docker",
        image: "docker:18.09.8-dind",
        securityContext: [
            privileged: true
        ],
        resources: [
            limits: [ memory: "1.5Gi" ],
            requests: [ memory: "256Mi" ]
        ],
        command: ["dockerd-entrypoint.sh", "--storage-driver=overlay2"]
    ]
}

def buildPod(){
  return pod(docker())
}

pipeline {
    agent {
        kubernetes {
            yaml buildPod()
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