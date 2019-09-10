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
	}
}