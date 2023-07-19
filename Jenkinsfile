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
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://index.docker.io/v1/', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploying myapp-deploy to Kubernetes') {
      steps {
        script {
          kubernetesDeploy (configs:'myapp-deploy.yml', kubeconfigId:'k8s-credentials' )
        }
      }
    }
  }
}