
def remote = [:]
    	remote.name = 'deploy'
    	remote.host = '172.16.10.25'
    	remote.user = 'root'
    	remote.password = 'vagrant'
    	remote.allowAnyHosts = true
def remote1 = [:]
    	remote1.name = 'prod'
    	remote1.host = '172.16.10.22'
    	remote1.user = 'ansible'
    	remote1.password = 'ansible'
    	remote1.allowAnyHosts = true
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
		        // sshScript remote: remote, script: "abc.sh"  	
			sshPut remote: remote, from: 'target/java-maven-1.0-SNAPSHOT.war', into: '/home/vagrant/tomcat/webapps'		        
		    }
    	 }
   stage('Remote SSH') {
	   steps {
      sshCommand remote: remote1, command: "cd ~"
      sshCommand remote: remote1, command: "git clone https://github.com/narendrasai316/ansible-files.git"
	  }
	   }
  
   
  stage('Deploy-to-prod') {
		     
		    steps {
		        // sshScript remote: remote, script: "abc.sh"  	
			sshPut remote: remote1, from: 'target/java-maven-1.0-SNAPSHOT.war', into: '/home/ansible/ansible-files/Ansible Roles/tomcat/files/'		        
		    }
    	 } 
   stage('Remote SSH1') {
	   steps {
      sshCommand remote: remote1, command: "ansible webservers -m ping"
      sshCommand remote: remote1, command: "ansible-playbook /home/ansible/ansible-files/Ansible\ Roles/tomcat.yml"
	  }
	   }
  }
}
