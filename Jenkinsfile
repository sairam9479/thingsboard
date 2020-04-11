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
                mvn clean install -DskipTests=true -Ddockerfile.skip=false -Dlicense.skip=true -DblackBoxTests.skip=true
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
def notifyInitialize() {
        slackSend "Initialized - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        channel: "gv-cicd-commits"
        tocken: "kwnUv7Bb1sHaOcD2lRkWstmd"
def notifyBuild() {
        slackSend "Built Images - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        channel: "gv-cicd-builds"
        tocken: "6gcqDQwcSU3JzdO34ZySoB5Y"   
    }
def notifyTest() {
        slackSend "Test - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        channel: "gv-cicd-commits"
        tocken: "v4BpmVKeFOuTI731evpDPozf"   
    }
def notifyDockerDeploy() {
            slackSend (color: "good", message: "Latest docker images deployed to 192.168.7.228:5000", channel: "gv-cicd-deployments", tocken: "8VGlLZj1K7pctnppdATZluTy")
        }
}


