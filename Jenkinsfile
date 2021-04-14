pipeline {
  environment {
    registry = "rcastelani/teste-jenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('CleanWorkspace') {
      steps {
        cleanWs()
      }
    }
    stage('Cloning Git') {
      steps {
        git 'https://github.com/rcastelani/k8s-lab.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          sh "hostname -I"
          sh "pwd"
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
  }
}