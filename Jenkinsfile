pipeline {
	
   	agent any
	stages {
	    stage('Change working directory...') {
                        steps {
                dir('docker') {
                    sh 'pwd'
                }
                        }
        }
        stage ('deploy') {
            steps{
	        sh 'pwd'
                sh 'bash ./docker-start-services.sh' 
		    }
        }
		
	}
}





