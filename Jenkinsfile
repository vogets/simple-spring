pipeline {
  environment {
    registryCredential = "docker-credentials"
  }
  agent any
  tools {
      jdk 'JDK8'
      maven 'maven'
      dockerTool 'myDocker'
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
stage("SSH Into k8s Server") {
        def remote = [:]
        remote.name = 'master'
        remote.host = '192.168.26.10'
        remote.user = 'vagrant'
        remote.password = 'vagrant'
        remote.allowAnyHosts = true

        stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
            sshPut remote: remote, from: 'k8s-spring-boot-deployment.yml', into: '.'
        }

        stage('Deploy spring boot') {
          sshCommand remote: remote, command: "kubectl apply -f k8s-spring-boot-deployment.yml"
        }
    }
  }
}
