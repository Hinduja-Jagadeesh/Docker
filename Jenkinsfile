pipeline{
	agent any
	stages{
		stage ('sonar test'){
			agent{
				docker{
					image 'maven'
					args '-v $HOME/.m2:/root/.m2'
				}
			}
			steps{
				echo 'hello'
			}
		}
	}
}
				
			
