pipeline {
  environment {
    registryCredential = "docker-credentials"
  }
  agent any
  tools {
      jdk 'JDK8'
      maven 'maven'
    }
  stages {
    stage(‘Build’) {
      steps{
        script {
          sh 'mvn clean install'
        }
      }
    }
    stage(‘Load’) {
      steps{
        script {
          app = docker.build("vogetisameer12/simple-spring")
        }
      }
    }
    stage(‘Initialize’){
            def dockerHome = tool 'myDocker'
            env.PATH = "${dockerHome}/bin:${env.PATH}"
        }
     stage(‘Deploy’) {
      steps{
        script {
          docker.withRegistry( "https://registry.hub.docker.com", registryCredential ) {
          //dockerImage.push()
          app.push("latest")
          }
        }
      }
    }
    stage('Deploy to K8s') {
       steps {
        withKubeConfig([credentialsId: 'kubernetes-config']) {
          sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'
          sh 'chmod u+x ./kubectl'
          sh './kubectl apply -f k8s.yaml'
        }
      }
    }
  }
}
