#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def toolbelt = tool 'toolbelt'
    def HUB_ORG="s.inuganti-zxkv@force.com"
    def SFDC_HOST = "https://test.salesforce.com/"
    def CONNECTED_APP_CONSUMER_KEY = "3MVG9t0sl2P.pByryftQdHSRVMZMNwYNXQ2BNSv30mEW9rSyuSSIA3t0v82DQgtBQstJpYy3x_lJ_hpex47ul"
    stage('checkout source') {
        checkout scm
    }
    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code'){
        rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
        if (rc != 0) { error 'hub org authorization failed' }
		println rc
		rmsg = sh returnStdout: true, script: "${toolbelt} force:source:deploy -d manifest/. -u ${HUB_ORG}"	
        }
    }
}
