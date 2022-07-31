node 
{
    def mavenHome = tool name: "maven3.8.6"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    echo "the job name is: ${env.JOB_NAME}"

    try{
        
    stage ('checkoutcode'){
    git branch: 'development', credentialsId: '16daf2af-3e61-42f3-a599-f35448fa3015', url: 'https://github.com/keerthisimha/maven-web-application.git'
}
    stage ('Build'){
    sh "${mavenHome}/bin/mvn clean package"
}   
    /*
    stage ('executeSonarqubeReport'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
}
    stage ('UploadArtifactsInToNexus'){
    sh "${mavenHome}/bin/mvn deploy"
}
    stage ('deployappintoTomcatserver'){
    sshagent(['849c3306-6a60-4763-abb7-da7047242fac']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.47.168:/opt/apache-tomcat-9.0.64/webapps"
}
}
 */
} 
catch(e){
currentBuild.result = "FAILURE"
throw e
}
finally{
slacknotification(currentBuild.result)
}
}// node closing

def slacknotification(String buildStatus = 'STARTED') {
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
 


