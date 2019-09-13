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
    stage('static-analysis') {
      steps {
        sh 'mvn cobertura:cobertura'
        cobertura(coberturaReportFile: '**target/site/cobertura/coverage.xml')
      }
    }
    stage('build') {
      steps {
        realtimeJUnit(testResults: 'target/surefire-reports/*.xml') {
          sh 'mvn clean deploy'
        }

      }
    }
  }
}