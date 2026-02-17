pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Pruebas de SAST') {
      steps {
        bat 'echo Ejecución de pruebas de SAST'
      }
    }

    stage('Build') {
      steps {
        bat 'docker build -t devops_ws .'
      }
    }
  }
}
