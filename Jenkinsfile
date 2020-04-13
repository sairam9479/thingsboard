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
                sh "mvn clean pre-integration-test -DskipTests -Dlicense.skip -Ddockerfile.skip=false "
            }
		}
		
	}
}
