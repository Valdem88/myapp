pipeline {
  environment {
    dockerimagename = "valdem88/myapp"
    dockerImage = ""
    
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/Valdem88/myapp.git'
      }
    }
    stage('Checkout tag') {
      steps{
        script {
          gitTag=sh(returnStdout: true, script: "git describe --tags --abbrev=0").trim()
          echo "gitTag output: ${gitTag}"
        }
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image:tags') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://index.docker.io/v1/', registryCredential ) {
            dockerImage.push("${gitTag}")
          }
        }
      }
    }
  }    
}    