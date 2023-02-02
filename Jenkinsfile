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
				echo 'archiving...'
				archiveArtifacts artifacts: '**/*.war'
			}
		}
	}
}
