pipeline {
	agent any
	stages{
		stage('Build'){
			steps{
				sh 'mvn -s settings.xml -Dskiptests install'
			}
		}
	}
}
