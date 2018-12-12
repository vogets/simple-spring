pipeline {
  environment {
    registryCredential = "dockerhub"
  }
  agent any
  stages {
    stage(‘Build’) {
      steps{
        script {
          app = docker.build("achyuth007/simple-spring")
        }
      }
    }
     stage(‘Deploy’) {
      steps{
        script {
          docker.withRegistry( "https://registry.hub.docker.com", registryCredential ) {
           // dockerImage.push()
          app.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }
    stage('Deploy to ACS'){
      steps{
          withCredentials([azureServicePrincipal('principal-credentials-id')]) {
            sh 'az login --service-principal -u c5ceb42a-033d-4dcf-bc2b-b2a7b37bff21 -p xyeBmx1bynF2Z6T+dzCgklfQ+1CuNPI6aY7EdIfE0OI= -t be10e06f-0415-4faf-8faf-d4ccf24c1ede'
            sh 'az account set -s 1e5fc2e8-f4df-4895-9f77-00e140031cb2'
            sh 'az resource list'
      }
    }
  }
  }
}
