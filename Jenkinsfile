pipeline{
	agent any
	stages{
		stage('BUILD'){
			steps{
				echo 'This Is the Build Steps';
				sh 'cd /root/myapps-training';
				sh 'jar -cvf dist/myapps.war index.html';
				milestone(2)
				archiveArtifacts artifacts: 'dist/myapps.war';
				}
			}
		stage('DEPLOY'){
			steps{
			
				withCredentials([usernamePassword(credentialsId: 'deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {

	sshPublisher(

			failOnError: true,
			continueOnError: false,
			publishers: [
				sshPublisherDesc(

					configName: 'qual',
					sshCredentials: [
						username: "$USERNAME",
						encryptedPassphrase: "$USERPASS"
				
							],
							transfers: [
								sshTransfer(
									sourceFiles: 'dist/myapps.war',
									removePrefix: 'dist/',
									remoteDirectory: '/tmp',
									execCommand: '/usr/share/apache-tomcat-9.0.48/bin/shutdown.sh & cp /tmp/myapps.war /usr/share/apache-tomcat-9.0.48/webapps/ & /usr/share/apache-tomcat-9.0.48/bin/startup.sh'
									)
								]
						)
					]				

			)
	}
	
				}
				}

		}
}
