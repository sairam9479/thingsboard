pipeline {
	agent any
	stages {
		stage ('build') {
            steps{
                sh 'mvn versions:set -DnewVersion=2.5.${BUILD_NUMBER}' 
			    sh 'mvn clean install -Dlicense.skip -DskipTests'
		    }
        }
		stage ('test') {
			steps{
                sh 'mvn versions:set -DnewVersion=2.5.${BUILD_NUMBER}' 
				sh 'mvn surefire-report:report -Dlicense.skip' 
				sh 'mvn surefire-report:report-only -Daggregate=true -Dlicense.skip' 
            }
		}
		stage ('docker build & push') {
			steps{
                sh 'mvn versions:set -DnewVersion=2.5.${BUILD_NUMBER}'
				sh "mvn clean pre-integration-test -DskipTests=true -Dlicense.skip=true -DblackboxTests.skip=true -Ppush-docker-image -Ddockerfile.skip=false -Ddocker.repo=localhost:5000/corinexgv:2.5.${BUILD_NUMBER}"
            }
		}
		
	}
}
