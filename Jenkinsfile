pipeline {
	
   	agent any
	stages {
                     
             stage ('deploy') {
            steps{
		    dir('docker') {
                    sh 'pwd'
                }
	        sh 'pwd'
                sh 'bash ./docker-start-services.sh' 
		    }
        }
		
	}
}





