def mvnHOME
def remote = [:]
		remote.name = 'deploy'
		remote.host = '192.168.33.59'
		remote.user = 'root'
		remote.password = 'vagrant'
		remote.allowAnyHosts = true
pipeline 
{
	agent none
	
        stages
        {
        	stage ('preparation') {
        		agent {
                 	label 'Slave'
                 }
                 steps {

                 git 'https://github.com/narendrasai316/Maven-Java-Project.git'
                 
                 script{
			        mvnHOME = tool 'maven3.6'
			    }
                 }
                 }

            stage ('static_analysis') {
            	agent {
                 	label 'Slave'
                 }
                 steps {
                    sh " '${mvnHOME}/bin/mvn' clean cobertura:cobertura "
                 }
                 post {
                 		success {
                 			cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
                 		}
                 }

            }
            stage ('build') {
            	agent {
                 	label 'Slave'
                 }
                 steps {
                 sh " '${mvnHOME}/bin/mvn' clean deploy "
		 sh " '${mvnHOME}/bin/mvn' -v "
		 sh " cd ~/.bashrc "
                 }
                 post {
                 	always {
                 		junit healthScaleFactor: 5.0, testResults: 'target/surefire-reports/*.xml'
                 		archiveArtifacts '**/*'
                 		fingerprint '**/*'
                 	}
                 }


            }
            stage ('deploy-to-container') {
            	agent {
                 label 'Slave'
                 }
                 steps {
                 	 sh " '${mvnHOME}/bin/mvn' clean package "
			 
			 sshPut remote: remote, from: 'target/java-maven-1.0-SNAPSHOT.war', into: '/root/workspace/tomcat/webapps'
                 }	
            }           
        }
}
