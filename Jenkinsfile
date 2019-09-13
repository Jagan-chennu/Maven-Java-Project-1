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
        sh 'mvn clean deploy'
        realtimeJUnit(testResults: '**/surefire-reports/*.xml', healthScaleFactor: 5)
      }
    }
  }
}