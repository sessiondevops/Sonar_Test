node('master') {
	checkout scm
	stage('build') {
		withMaven(jdk: 'Default Java', maven: 'Default Maven') {
			sh 'mvn clean install verify site'
		}
	}
	stage('SonarQube analysis') {
		def scannerHome = tool 'Sonar Scanner';
		withSonarQubeEnv('SonarQube') {
			sh "${scannerHome}/bin/sonar-scanner"
		}
	}
}
