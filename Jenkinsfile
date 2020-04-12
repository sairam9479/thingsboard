pipeline {
	
   	agent any
	stages {
	        
		
        stage ('deploy') {
            steps{
	        dir ('docker')
	{
		sh 'pwd'
	}
                sh 'bash docker/docker-start-services.sh ' 
		    }
        }
		
	}
}




