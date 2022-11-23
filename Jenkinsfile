pipeline {
	agent any
	tools{
		maven 'Maven'
		}
	stages {
		stage('Build'){
			steps {
				sh "mvn -X clean install"
			}
		}
		stage('test'){
			steps{
				sh 'mvn test'
		}
	}}

}
