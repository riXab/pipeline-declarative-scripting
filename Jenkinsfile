pipeline{
	agent any
	
	tools{
		jdk 'localJDK'   				
		maven 'localMaven'				
	}

	stages{
	
		stage('Build'){
			steps{
				bat script: 'mvn clean package'
			}
			post{
				success{
					echo 'Archiving the Artifact;'
					archiveArtifacts '**/*.war'
				
				}
				failure{
					echo 'Failed to Archive'
				}
			}
		}
	
	stage('Deploy to Staging'){
				steps{
					echo 'Deploying to Staging'
					deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:8989')], contextPath: null, war: '**/*.war'
				}
				post{
					success{
						echo 'Delpoyed to staging'
					}
					failure{
						echo 'Failed to deploy to staging'
					}
				}
	}
	
	}
	
}