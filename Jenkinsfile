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
        bat 'mvn clean install'
      }
    }

    stage('Generate Dockerfile') {
      steps {
          sh 'mvn dockerfile:build'
      }
    }
    
    stage('Docker Build') {
      steps {
        bat 'docker build -t joshtkh/maven-web-app:1.1 .'
      }
    }
    
    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
          bat "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
        }
      }
    }
    
    stage('Docker Push') {
      steps {
        bat 'docker push joshtkh/maven-web-app:1.1'
      }
    }
  }
}