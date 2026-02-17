@Library('obsschool-sharedlib') _

pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Pruebas de SAST') {
      steps {
        staticAnalysis(abortPipeline: false)
      }
    }

    stage('Build') {
      steps {
        bat 'docker build -t devops_ws .'
      }
    }
  }
}
