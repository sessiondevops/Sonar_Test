node('master') {
	checkout scm
	stage('build') {
		withMaven(jdk: 'Default Java', maven: 'Default Maven') {
			sh 'mvn clean install'
		}
	}
	stage('SonarQube analysis') {
		def scannerHome = tool 'Sonar Scanner';
		withSonarQubeEnv('SonarQube') {
			sh "${scannerHome}/bin/sonar-scanner"
		}
	}
}
