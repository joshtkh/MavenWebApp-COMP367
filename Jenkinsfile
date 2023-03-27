pipeline {
  agent any
  
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    
    stage('Build Maven Project') {
      steps {
        sh 'mvn clean install'
      }
    }
    
    stage('Docker Build') {
      steps {
        sh 'docker build -t joshtkh/MavenWebApp:${BUILD_NUMBER} .'
      }
    }
    
    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
          sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
        }
      }
    }
    
    stage('Docker Push') {
      steps {
        sh 'docker push joshtkh/MavenWebApp:${BUILD_NUMBER}'
      }
    }
  }
}