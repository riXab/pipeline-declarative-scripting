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
		
		stage('Parallel-Check-Deploy'){
			parallel {
				stage('Deploy to Staging'){
					steps{
						echo 'Deploying to Staging'
						deploy adapters: [tomcat8(credentialsId: 'fc29fbea-a440-4146-8bf0-dd8e0e199d22', path:'', url: 'http://localhost:8989')], contextPath: null, war: '**/*.war'
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
				stage('Build'){
					steps{
						bat script: 'checkstyle:checkstyle'
					}
					post{
						success{
							echo 'Checkstyle completed'
						}
					}
				}
			}
		}
	
		stage('Deploy to Production'){
			steps{
				echo 'Going Live - Manual approval required'
				timeout(2){
					input 'APprove Live Deployment?'
				 }
				deploy adapters: [tomcat8(credentialsId: 'fc29fbea-a440-4146-8bf0-dd8e0e199d22', path:'', url: 'http://localhost:8787')], contextPath: null, war: '**/*.war'
			}
			post{
				success{
					echo 'Delpoyed to production'
				}
				failure{
					echo 'Failed to deploy to production'
				}
			}
		}
		
	}
}