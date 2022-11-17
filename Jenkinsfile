pipeline {
	agent none
	stages {
		stage('DependencyCheck'){
			agent any
			stages {
				stage('OWASP DependencyCheck') {
					steps {
						dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'default'
					}
				}
			}	
			post {
				success {
					dependencyCheckPublisher pattern: 'dependency-check-report.xml'
				}
			}
		}

		stage('SonarQube'){
			agent any 
			stages {
				stage('Code Quality Check via SonarQube') { 
					steps {
						script {
							def scannerHome = tool 'SonarQube';
							withSonarQubeEnv('SonarQube') {
								sh "${scannerHome}/bin/sonar-scanner \
												-Dsonar.projectKey=ICT3103_2021 \
												-Dsonar.sources=. \
												-Dsonar.host.url=http://172.18.0.4:9000 \
												-Dsonar.login=1e2d8f368cc8039fb1a611b061305951e93ef617"
							}
						}
					}
				}
			} 
			post {
				always {
					recordIssues enabledForFailure: true, tool: sonarQube()
				} 
			}
		}
	}
}