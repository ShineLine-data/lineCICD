pipeline {
  agent any

  environment {
    FRONTEND_REPO = 'https://github.com/ShineLine-data/lineFrontend.git'
    BACKEND_REPO  = 'https://github.com/ShineLine-data/lineBackend.git'
  }

  stages {

    stage('Clone Repositories') {
      steps {
        sh 'rm -rf frontend backend'  // Nettoyage (si nécessaire)
        sh 'git clone $FRONTEND_REPO frontend'
        sh 'git clone $BACKEND_REPO backend'
      }
    }

    stage('Build and Deploy with Docker Compose') {
      steps {
        sh 'docker compose down || true' // Arrête tout si déjà lancé
        sh 'docker compose build'
        sh 'docker compose up -d'
      }
    }

    stage('Check App Availability') {
      steps {
        echo "Waiting for the frontend to be ready..."
        sh 'sleep 10' // Simple attente, mieux que rien
        sh 'curl -sSf http://localhost:3000 || exit 1'
      }
    }

  }

  post {
    always {
      echo 'Pipeline terminé'
    }
  }
}
