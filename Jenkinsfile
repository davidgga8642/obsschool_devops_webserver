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
            script {
              def scannerHome = tool 'SonarScannerCLI'

              withSonarQubeEnv('SonarQube') {
                bat "\"${scannerHome}\\bin\\sonar-scanner.bat\""
              }

              timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: false
              }
            }
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
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          bat """
            echo [credentials] > credentials.ini
            echo user=%USER% >> credentials.ini
            echo password=%PASS% >> credentials.ini
          """
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
      archiveArtifacts artifacts: 'credentials.ini', fingerprint: true, onlyIfSuccessful: false
    }
  }
}
