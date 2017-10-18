node('master') {
	checkout scm
	stage('build') {
		withMaven(jdk: 'Default Java', maven: 'Default Maven') {
			sh 'mvn clean install verify site'
		}
	}
	stage('SonarQube analysis') {
		if(env.BRANCH_NAME == 'master') {
			def scannerHome = tool 'Sonar Scanner';
			withSonarQubeEnv('SonarQube') {
				sh "${scannerHome}/bin/sonar-scanner"
			}
		}
	}
	stage('Artifactory upload') {
		if(env.BRANCH_NAME == 'master') {
			def server = Artifactory.server 'Default Artifactory'
			def uploadSpec = """{
				"files": [
					{
						"pattern": "target/helloworld-example-0.1.0.jar",
						"target": "helloworld-greeting-project/${BUILD_NUMBER}/",
						"props": "Integration-Tested=Yes;Performance-Tested=No"
					}
				]
			}""" server.upload(uploadSpec)
		}
	}
}
