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
	stage('Artifactory download and upload') {
		if(env.BRANCH_NAME == 'master') {
			steps {
				script{
					// Obtain an Artifactory server instance, defined in Jenkins --> Manage:
					def server = Artifactory.server SERVER_ID

					// Read the download and upload specs:
					def downloadSpec = readFile 'jenkins-pipeline-examples/resources/props-download.json'
					def uploadSpec = readFile 'jenkins-pipeline-examples/resources/props-upload.json'

					// Download files from Artifactory:
					def buildInfo1 = server.download spec: downloadSpec
					// Upload files to Artifactory:
					def buildInfo2 = server.upload spec: uploadSpec

					// Merge the local download and upload build-info instances:
					buildInfo1.append buildInfo2

					// Publish the merged build-info to Artifactory
					server.publishBuildInfo buildInfo1
				}
			}
		}
	}
}
