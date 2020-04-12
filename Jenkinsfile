pipeline {
	
   	agent any
	environment {
       WORKSPACE = "/var/lib/jenkins/workspace/deployment/docker"
    }

	stages {
                     
             stage ('deploy') {
            steps{
		sh 'pwd'
		    sh 'bash ${WORKSPACE}/docker-start-services.sh' 
		    }
        }
		
	}
}





