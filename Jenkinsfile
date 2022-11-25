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
        }
                stage('sonaranalysis'){
						 environment {
							scannerHome = tool 'mysonar'
								}
                        steps{
							
								
								withSonarQubeEnv('mysonar'){
									sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=sonar-pro -Dsonar.projectName=sonar-pro"
								}
							
							}
							}
				stage('Qualitygate'){
						steps{
							script{
								def qualitygate = waitForQualityGate()
								sleep(10)
								if(qualitygate.status != "OK"){
									waitForQualityGate abortPipeline: true
								}
							}
						}
				}
						



}


}
