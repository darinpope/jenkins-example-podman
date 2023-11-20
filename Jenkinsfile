pipeline {
  agent any
  environment {
    DH_CREDS=credentials('dh-creds')
  }
  stages {
    stage('Remove all images from agent') {
      steps {
        sh 'podman rmi --all --force'
      }
    }
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
        sh 'podman tag darinpope/hello-world:2023-11-18 darinpope/hello-world:latest'
      }
    }
    stage('Push the image') {
      steps {
        sh '''
          podman push darinpope/hello-world:2023-11-18
          podman push darinpope/hello-world:latest
        '''
      }
    }
  }
  post {
    always {
      sh 'podman logout docker.io'
    }
  }
}