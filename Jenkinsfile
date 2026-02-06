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
          }
        }
      }
    }

    stage('Configurar archivo') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'Credentials_DevOps',
          usernameVariable: 'DEVOPS_USER',
          passwordVariable: 'DEVOPS_PASS'
        )]) {
          bat '''
            @echo off
            echo [credentials]> credentials.ini
            echo user=%DEVOPS_USER%>> credentials.ini
            echo password=%DEVOPS_PASS%>> credentials.ini
          '''
        }
      }
    }

    stage('Build') {
      steps {
        bat 'docker build -t devops_ws .'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'credentials.ini', fingerprint: true
    }
  }
}
