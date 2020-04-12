node {
    checkout scm
	env.commitMsg= sh (returnStdout: true,script: "git log -1").trim()
 	env.branchname= sh (returnStdout: true,script: "git name-rev --name-only HEAD").trim()
	notifyCheckoutSuccessful()
        def mvnHome = tool name: 'Maven'

    /* Set JAVA_HOME, and special PATH variables. */
    List Env = [
        "MIB_REP_PATH=${WORKSPACE}/application/src/main/resources/mibs/",
        "M2_HOME=${mvnHome}"
    ]

    withEnv(Env) {
		stage ('Initialize') {
			sh '''
				echo "PATH = ${PATH}"
				echo "M2_HOME = ${M2_HOME}"
			'''
		}
       
    }
		stage ('Build') {
			try {
				sh 'mvn clean install -Dlicense.skip -DskipTests'
                                sh 'mvn clean install -DskipTests=true -Ddockerfile.skip=false -Dlicense.skip=true -DblackBoxTests.skip=true'
			} catch (e) {
				currentBuild.result = 'FAILURE'
			}
		}
        
		
		stage ('Test') {
			try {
				sh 'mvn surefire-report:report -Dlicense.skip'
				sh 'mvn surefire-report:report-only -Daggregate=true -Dlicense.skip'
				} catch (e) {
				currentBuild.result = 'FAILURE'
			}
		}
        
		stage ('Docker Deploy') {
			try {
				sh "mvn clean pre-integration-test -DskipTests -Dlicense.skip -Ppush-docker-image -Ddockerfile.skip=false -Ddocker.repo=192.168.7.228:5000/corinexgv"         
				notifyDockerDeploy()
			} catch (e) {
				currentBuild.result = 'FAILURE'
			}
        }    
	}
      try {
        notifyBuild('STARTED')

        stage('Prepare code') {
            echo 'do checkout stuff'
        }

        stage('Testing') {
            echo 'Testing'
            echo 'Testing - publish coverage results'
        }

        stage('Staging') {
            echo 'Deploy Stage'
        }

        stage('Deploy') {
            echo 'Deploy - Backend'
            echo 'Deploy - Frontend'
        }

  } catch (e) {
    // If there was an exception thrown, the build failed
    currentBuild.result = "FAILED"
    throw e
  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)
  }
}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
}


