pipeline {
	
   	agent any
	environment {
       WORKSPACE = "/var/lib/jenkins/workspace/deployment/docker"
    }

	stages {
                     
             stage ('deploy') {
            steps{
		    dir('docker')
		    sh 'pwd'
                sh 'bash ./docker-start-services.sh' 
		    }
        }
		
	}
}





