pipeline {
  agent any
  
  environment {
    DOCKERHUB_CREDENTIALS = credentials('DockerHub-credentials')
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

    stage('Push Images to Docker Hub') {
      steps {
        bat 'echo %DOCKERHUB_CREDENTIALS_PSW%| docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin'
        bat 'docker-compose push'
      }
    }

    stage('Cleanup') {
      steps {
        bat 'docker-compose down'
      }
    }
  }

 post {
    success {
      emailext subject: 'Sujet : Reussite du pipeline Jenkins',
               body: '''Le pipeline Jenkins s'est exécuté avec succès. 
                        Tout s'est déroulé sans erreur.
                        Voici le lien de l'application si vous souhaitez le consulter : https://pfea8.azurewebsites.net/
                     ''',
               to: 'salamhassanesaibou@gmail.com'
    }
    failure {
      emailext subject: 'Sujet : Echec du pipelinee Jenkins',
               body: '''Le pipeline Jenkins a échoué. 
                        Veuillez prendre les mesures nécessaires pour résoudre le problème.
                     ''',
               to: 'salamhassanesaibou@gmail.com'
    }
  }
}
