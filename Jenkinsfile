def COLOR_MAP = [
	'SUCCESS': 'good',
	'FAILURE': 'danger'
]
pipeline{
	agent any
	tools{
		maven "MAVEN3"
		/*jdk "OracleJDK8" */
	}
	environment{
		SNAP_REPO = 'vprofile-snapshot'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
		NEXUS_GRP_REPO = 'vpro-maven-group'
		NEXUS-USER = 'admin'
		NEXUS-PASS = 'admin'
		NEXUSIP = '172.31.16.125'
		NEXUSPORT = '8081'
		NEXUSLOGIN = 'nexuslogin' 
		SONARSERVER = 'sonar'
		SONARSCANNER = 'sonar'
	}
	stages{
		stage('Build'){
			steps{
				sh 'mvn -s settings.xml -DskipTests install'
			}
			post{
				success{
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/*.war'
				}
			}
		}
		stage('Test'){
			steps{
				sh 'mvn -s settings.xml test' 			}
		}
		stage('Sonar Analysis'){
			environment{
				scannerHome = tool "${SONARSCANNER}"
			}
			steps{
				withSonarQubeEnv("${SONARSERVER}"){
				sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile-app \
					-Dsonar.projectName=vprofile-app \
					-Dsonar.projectVersion=1.0 \
					-Dsonar.sources=src/				
					-Dsonar.java.binaries=target/test-classes/com/etc/ 
					-Dsonar.junit.reportsPath=target/surefire-reports/ 
					-Dsonar.jacoco.reportsPath=target/jacoco.exec '''				}
					
			}		
		
		}
		stage('Quality Gate'){
			steps{
				timeout(time: 1, unit: 'HOURS'){
					waitForQualityGate abortPipeline: true
				}
			}
		}
		stage('Artifact Uploader'){
			steps{
				nexusArtifactUploader(
					nexusVersion: 'nexus3',
					protocol: 'http',
					nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
					groupId: QA,
					version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
					repository: "${RELEASE_REPO}",
					credentialsId: "${NEXUSLOGIN}",
					artifacts: [
						[artifactId: 'vproapp',
						 classifier: '',
						 file: 'target/vprofile-v2.war',
						 type: 'war']
					]
				)
			}
		}
					
		
	}
	post{
		always{
			echo 'SlackNotifications.'
			/*slackSend channel: '#channelname',
			color: COLOR_MAP[currentBuild.currentResult],
			message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"*/
		}
	}
	
}				
