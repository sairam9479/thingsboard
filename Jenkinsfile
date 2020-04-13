pipeline {
	agent any
	stages {
	stage ('build') {
            steps{
                sh 'mvn versions:set -DnewVersion=2.5.${BUILD_NUMBER}' 
		sh 'mvn clean install -Dlicense.skip -DskipTests'
		    }
        }
		
       stage ('docker build & push') {
	    steps{
                sh "mvn clean pre-integration-test -DskipTests -Dlicense.skip -Ppush-docker-image -Ddockerfile.skip=false -Ddocker.repo=192.168.7.228:5000/corinexgv:2.5.${BUILD_NUMBER}"
            }
		}
		
	}
}
