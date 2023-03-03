#!groovy

pipeline {
  agent none
  stages {
    stage('Build & Package') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn package -DskipTests'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t alltimejk/spring-petclinic:${GIT_COMMIT:0:7} .'
        sh 'docker build -t alltimejk/spring-petclinic:latest .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DockerHubPassword', usernameVariable: 'DockerHubUser')]) {
          sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPassword}"
          sh 'docker push alltimejk/spring-petclinic:${GIT_COMMIT:0:7}'
          sh 'docker push alltimejk/spring-petclinic:latest'
        }
      }
    }
  }
}
