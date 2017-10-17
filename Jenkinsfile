node('master') {
	checkout scm
	stage('build') {
		withMaven(jdk: 'Default Java', maven: 'Default Maven') {
			sh 'mvn clean install'
		}
	}
	stage('SonarQube analysis') {
		def scannerHome = tool 'SonarQube Scanner 2.8';
		withSonarQubeEnv('SonarQube') {
			sh "${scannerHome}/bin/sonar-scanner"
		}
	}
}
