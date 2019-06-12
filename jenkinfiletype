def mvnHOME
def remote = [:]
		remote.name = 'deploy'
		remote.host = '35.154.113.73'
		remote.user = 'root'
		remote.password = 'stage'
		remote.allowAnyHosts = true
pipeline 
{
	agent none
	
        stages
        {
        	stage ('preparation') {
        		agent {
                 	label 'slave'
                 }
                 steps {

                 git 'https://github.com/narendrasai316/Maven-Java-Project.git'
                 stash 'source'
                 script{
			        mvnHOME = tool 'maven3.6'
			    }
                 }
                 }

            stage ('static_analysis') {
            	agent {
                 	label 'slave'
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
                 	label 'slave'
                 }
                 steps {
                 sh " '${mvnHOME}/bin/mvn' clean deploy "
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
                 label 'slave'
                 }
                 steps {
                 		sshPut remote: remote, from: 'target/java-maven-1.0-SNAPSHOT.war', into: '/u01/stage_server/webapps'
                 }	
            }           
        }
}
