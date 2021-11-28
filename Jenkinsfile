#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def toolbelt = tool 'toolbelt'
    def HUB_ORG="s.inuganti-zxkv@force.com"
    def SFDC_HOST = "https://login.salesforce.com/"
    def SECURITY_TOKEN = "0AFTBXuZsvXgyTCvyxtRTbeWR"
	def PASSWORD = 'Padmaja@2021'
    stage('checkout source') {
        checkout scm
    }
    withCredentials([file(credentialsId: "df75448b-67c4-40b2-ac90-7dec9a585372", variable: 'jwt_key_file')]) {
        stage('Deploye Code'){
	    rc = bat returnStdout: true, script: "\"${toolbelt}\" plugins:install sfpowerkit"
        rc1 = bat returnStatus: true, script: "\"${toolbelt}\" sfpowerkit:auth:login -u ${HUB_ORG} -p ${PASSWORD} -r ${SFDC_HOST} -s ${SECURITY_TOKEN}"
        if (rc1 != 0) { error 'hub org authorization failed' }
		println rc1
		rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy -p force-app/main/default/. -u ${HUB_ORG}"	
        }
    }
}
