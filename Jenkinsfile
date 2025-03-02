#!groovy

import groovy.json.JsonSlurperClassic

node {

    def BUILD_NUMBER=env.SF_CONSUMER_KEY
    def RUN_ARTIFACT_DIR=  "tests/${BUILD_NUMBER}"
    def SFDC_USERNAME
   
  def HUB_ORG=env.HUB_ORG_DH
  def SFDC_HOST =env.SFDC_HOST_DH
  def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
  def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
 
  println 'KEY IS'
  println  JWT_KEY_CRED_ID
  println  HUB_ORG
  println SFDC_HOST
  PRINTLN CONNECTED_APP_CONSUMER_KEY

    def toolbelt = tool 'toolbelt'

stage('checkout source'){
  checkout scm
}

withcredentials([file(credentialsid: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]){

stage('Deploye Code'){

if(isUnix()){
  rc = sh returnStatus: true, script: "${toolbelt}  force:auth:jwt:grant --ClientID ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
}
else{
   rc =bat returnStatus:true, scrip: "\"${toolbelt}\" force:auth:jwt:grant --ClientID ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdeafultdevhubusername --instanceurl ${SFDC_HOST}"
}
if (rc != 0){error 'hub org authorization failed'}

print rc
if(isUnix()){
rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
}
else
{
 rmsg = bat returnStdout:true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
}
printf rmsg
println('hello from a job dsl script')
println(rmsg)
}

}
}
