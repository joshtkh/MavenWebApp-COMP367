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
        bash 'mvn clean install'
      }
    }
    
    stage('Docker Build') {
      steps {
        bash 'docker build -t my-docker-username/my-maven-web-app:${BUILD_NUMBER} .'
      }
    }
    
    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
          bash "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
        }
      }
    }
    
    stage('Docker Push') {
      steps {
        bash 'docker push joshtkh/MavenWebApp:${BUILD_NUMBER}'
      }
    }
  }
}