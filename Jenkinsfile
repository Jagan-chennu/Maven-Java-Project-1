pipeline {
  agent none
  stages {
    stage('preparation') {
      agent {
        label 'Slave'
      }
      steps {
        git 'https://github.com/narendrasai316/Maven-Java-Project.git'
        script {
          mvnHOME = tool 'maven3.6'
        }

      }
    }
    stage('static_analysis') {
      agent {
        label 'Slave'
      }
      post {
        success {
          cobertura(autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false)

        }

      }
      steps {
        sh " '${mvnHOME}/bin/mvn' clean cobertura:cobertura "
      }
    }
    stage('build') {
      agent {
        label 'Slave'
      }
      post {
        always {
          junit(healthScaleFactor: 5, testResults: 'target/surefire-reports/*.xml')
          archiveArtifacts '**/*'
          fingerprint '**/*'

        }

      }
      steps {
        sh " '${mvnHOME}/bin/mvn' clean deploy "
        sh " '${mvnHOME}/bin/mvn' -v "
      }
    }
    stage('deploy-to-container') {
      agent {
        label 'Slave'
      }
      steps {
        sh " '${mvnHOME}/bin/mvn' clean package "
        sh ' scp /home/vagrant/Jenkin_Practise/workspace/ocean_blue_master/target java-maven-1.0-SNAPSHOT.war root@192.168.33.59:/root/workspace/tomcat/webapps '
      }
    }
  }
}