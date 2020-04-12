pipeline {
	dir ('docker')
	{
		sh 'pwd'
	}
   	agent any
	stages {
	        
		
        stage ('deploy') {
            steps{
	        
                sh 'bash docker-start-services.sh ' 
		    }
        }
		
	}
}




