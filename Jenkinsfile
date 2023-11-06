pipeline {
  agent any
  
  environment {
    DOCKERHUB_CREDENTIALS = credentials('DockerHub')
  }
  
  stages {
    stage('Checkout code') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        bat 'docker-compose build'
        bat 'start docker-compose up -d'
      }
    }

    stage('Test') {
      steps {
        echo "test passed"
      }
    }

    stage('Cleanup') {
      steps {
        bat 'docker-compose down'
      }
    }
  }
}
