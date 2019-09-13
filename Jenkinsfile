pipeline {
  agent {
    node {
      label 'slave'
    }

  }
  stages {
    stage('preparation') {
      steps {
        sh 'mvnHome = \'tool maven 3.6\''
      }
    }
  }
}