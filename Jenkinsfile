pipeline
dir('docker') {
                    sh 'pwd'
                } {
	
   	agent any
	stages {
                     
             stage ('deploy') {
            steps{
		    
	        sh 'pwd'
                sh 'bash ./docker-start-services.sh' 
		    }
        }
		
	}
}





