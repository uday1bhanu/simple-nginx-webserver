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
        allowPrivilegeEscalation: true
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
                dir('.') {
                    container('docker') {
                        sh "docker build --no-cache --network host -t uday1bhanu/simple-nginx-webserver:${env.BUILD_NUMBER}"
                        sh "docker push uday1bhanu/simple-nginx-webserver:${env.BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}