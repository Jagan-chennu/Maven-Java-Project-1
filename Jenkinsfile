pipeline {
  agent {
    node {
      label 'slave'
    }

  }
  stages {
    stage('preparation') {
      steps {
        sh 'mvn -v'
      }
    }
  }
  environment {
    mvnHome = '/opt/maven'
  }
}