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
        withCredentials([usernamePassword(credentialsId: 'Credentials_DevOps',
                                          usernameVariable: 'USER',
                                          passwordVariable: 'PASS')]) {
          bat '''
            echo [credentials] > credentials.ini
            echo user=%USER% >> credentials.ini
            echo password=%PASS% >> credentials.ini
            type credentials.ini
          '''
        }
      }
    }

    stage('Build') {
      steps {
        bat 'docker build -t devops_ws .'
      }
    }

    stage('Despliegue del servidor') {
      steps {
        bat '''
          docker stop devops || exit /b 0
          docker rm devops || exit /b 0
          docker run -d -p 8090:8090 --name devops devops_ws
          docker ps --filter "name=devops"
        '''
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'credentials.ini', fingerprint: true, onlyIfSuccessful: false
    }
  }
}
