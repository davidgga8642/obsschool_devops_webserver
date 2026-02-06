pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Pruebas de SAST + Imprimir Env') {
      parallel {
        stage('Pruebas de SAST') {
          steps {
            echo 'Ejecución de pruebas de SAST'
          }
        }

        stage('Imprimir Env') {
          steps {
            echo "WORKSPACE: ${env.WORKSPACE}"
            // Alternativa también válida:
            // sh 'echo "WORKSPACE: $WORKSPACE"'   // (en Linux)
            // bat 'echo WORKSPACE: %WORKSPACE%'  // (en Windows)
          }
        }
      }
    }

    stage('Build') {
      steps {
        // Si estás en Windows y tienes Docker Desktop funcionando:
        bat 'docker build -t devops_ws .'
        // Si fuera Linux:
        // sh 'docker build -t devops_ws .'
      }
    }
  }
}
