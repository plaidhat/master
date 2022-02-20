podTemplate(containers: [
	containerTemplate(
		name: 'gradle',
		image: 'gradle:6.3-jdk14', command: 'sleep',
		args: '30d'
		),
	]
) {
	node(POD_LABEL) {
		stage('Run pipeline against a gradle project') {
			git 'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
			container('gradle') {
				stage('Build a gradle project') {
					sh '''
					cd Chapter08/sample1 chmod +x gradlew ./gradlew test
					'''
				}
				stage("Code coverage") {
					try {
						sh '''
						pwd
						cd Chapter08/sample1
						mkdir -p build/reports
						./gradlew jacocoTestCoverageVerification
						./gradlew jacocoTestReport
						'''
					} catch (Exception E) {
						sh '''
						cd Chapter08/sample1
						mkdir -p build/reports
						echo 'Failure detected'
						./gradlew jacocoTestReport
						'''
					}
					publishHTML (target: [
						reportDir: 'Chapter08/sample1/build/reports/', reportFiles: 'index.html',
						reportName: "JaCoCo Report"
					])
				}
			}
		}
	}
}
