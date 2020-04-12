pipeline {
   	agent any
	stages {
	    
		stage ('deploy') {
            steps{
		sh 'cd docker'
                sh 'bash docker-start-services.sh ' 
		    }
        }
		
	}
}




