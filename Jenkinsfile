pipeline {
  agent any
  environment {
    DH_CREDS=credentials('dh-creds')
  }
  stages {
    stage('build image') {
      steps {
        sh 'podman build -t darinpope/hello-world:2023-11-18 .'
      }
    }
    stage('Login to Docker Hub') {
      steps {
        sh 'echo $DH_CREDS_PSW | podman login -u $DH_CREDS_USR --password-stdin docker.io'
      }
    }
    stage('Tag the image') {
      steps {
        sh 'podman tag darinpope/helloworld:2023-11-18 darinpope/hello-world:latest'
      }
    }
    stage('Push the image') {
      steps {
        sh '''
          podman push docker.io/darinpope/hello-world:2023-11-18
          podman push docker.io/darinpope/hello-world:latest
        '''
      }
    }
  }
  post {
    always {
      sh 'podman logout'
    }
  }
}