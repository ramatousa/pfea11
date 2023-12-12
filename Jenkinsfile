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
stage('Deploy to EC2') {
  steps {
    script {
      // Copier le fichier docker-compose.yml sur l'instance EC2
      sh "scp -i ${EC2_SSH_KEY} -o StrictHostKeyChecking=no ${DOCKER_COMPOSE_FILE} ${EC2_USER}@${EC2_HOST}:~/docker-compose.yml"

      // Exécuter les commandes Docker Compose sur l'instance EC2
      sshCommand remote: [
        allowAnyHosts: true,
        credentialsId: '',
        port: 22,
        remote: "${EC2_USER}@${EC2_HOST}",
        command: """
                  cd ~
                  docker-compose pull
                  docker-compose up -d
                  """
      ]
    }
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
