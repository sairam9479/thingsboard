pipeline {
   	agent any
	stages {
	    
		stage ('deploy') {
            steps{
                sh 'bash docker/docker-start-services.sh ' 
		    }
        }
		
	}
}




