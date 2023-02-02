pipeline {
	agent any
	tools{
		maven "maven"
	}
	stages{
		stage('Build'){
			steps{
				echo 'test build'
			}
			post{
				success{
					echo 'archiving...'
					archiveArtifacts artifacts: '**/*.war'
				}
			}

		}
	}
}
