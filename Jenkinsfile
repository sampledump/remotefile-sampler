def props = readRemoteProperties url:"https://raw.githubusercontent.com/spring-projects/spring-data-build/3.0.x/ci/configuration.properties"
def creds = props['artifactory.credentials.ref']
pipeline {

	agent none

	triggers {
		pollSCM 'H/10 * * * *'
	}

	options {
		disableConcurrentBuilds()
		buildDiscarder(logRotator(numToKeepStr: '14'))
	}

	stages {

		stage("test: baseline (Java 17)") {
			when {
				beforeAgent(true)
				anyOf {
					branch(pattern: "main|(\\d\\.\\d\\.x)", comparator: "REGEXP")
					not { triggeredBy 'UpstreamCause' }
				}
			}
			environment {
				ARTIFACTORY = credentials("${props['artifactory.credentials.ref']}")
			}
			agent any
			options { timeout(time: 30, unit: 'MINUTES') }

			steps {
				script {
							sh 'echo ${ARTIFACTORY} > tmp'
							sh "cat tmp"
							sh 'echo single ${creds}'
							sh "echo double ${creds}"
				}
			}
		}
	}
}
