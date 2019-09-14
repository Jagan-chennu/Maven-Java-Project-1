
def remote = [:]
    	remote.name = 'deploy'
    	remote.host = '172.16.10.25'
    	remote.user = 'root'
    	remote.password = 'vagrant'
    	remote.allowAnyHosts = true
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
        junit(testResults: '**/target/surefire-reports/*.xml', healthScaleFactor: 5)
      }
    }
   stage('Deploy-to-Stage') {
		     
		    steps {
		        //sshScript remote: remote, script: "abc.sh"  	
			sshPut remote: remote, from: 'target/java-maven-1.0-SNAPSHOT.war', into: '/home/vagrant/tomcat/webapps'		        
		    }
    	}
  }
}
