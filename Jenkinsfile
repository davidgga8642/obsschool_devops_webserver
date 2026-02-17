@Library('obsschool-sharedlib') _

pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Pruebas de SAST') {
      steps {
        staticAnalysis(branchName: 'main')
      }
    }

    stage('Build') {
      steps {
        bat 'docker build -t devops_ws .'
      }
    }
  }
}
