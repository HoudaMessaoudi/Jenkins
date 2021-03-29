pipeline {
  agent {
    node {
      label 'build'
    }

  }
  stages {
    stage('Build') {
      steps {
        bat 'gradle build'
      }
    }

  }
}