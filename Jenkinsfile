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
				emailext body: '${FILE,path="target/site/surefire-report.html"}',mimeType: 'text/html', replyTo: 'satchid.das@corinex.com', subject: '$DEFAULT_SUBJECT - Unit Test Results', to: 'satchid.das@corinex.com' 
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
}

def notifyCheckoutSuccessful() {
    slackSend (color: "good", message: "testing jenkins---CI started in jenkins ${env.branchname} :\n\n${env.commitMsg}", channel: "gv-cicd-deployments", tocken: "8VGlLZj1K7pctnppdATZluTy")
}

def notifyDockerDeploy() {
    slackSend (color: "good", message: "Testing jenkins---Latest docker images deployed to 192.168.7.228:5000", channel: "gv25-commits")
}

