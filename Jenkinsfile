node
{
    echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
    properties([
     buildDiscarder(logRotator(numToKeepStr: '3')),
     pipelineTriggers([
         pollSCM('* * * * *')
     ])
   ])
def mvnHome=tool name: "maven3.6.3"
	stage('CheckoutCode')
	{
	git branch: 'development', credentialsId: 'cac2dcd1-0724-486b-9515-b77ce216b678', url: 'https://github.com/globlesys/maven-web-application.git'
	}
	stage('Build')
	{
	sh "${mvnHome}/bin/mvn clean package"
	}
	stage('SonarQubeReport')
	{
	sh "${mvnHome}/bin/mvn sonar:sonar"
	}
	stage('UploadArtifactToNexus')
	{
	sh "${mvnHome}/bin/mvn clean deploy"
	}
	stage('DeployToTomcat')
	{
	sshagent(['1f4333ec-9246-47a5-bed9-e7abaca04403']) 
	{
	sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.114.118:/opt/apache-tomcat-9.0.30/webapps"
	}
	}
	stage('SentEmailNotification')
	{
	emailext body: '''$DEFAULT_CONTENT

	Thanks & regards,
	hemanth.M.''', subject: '$DEFAULT_SUBJECT-Mitun technologies', to: 'm.hemanth.redhat@gmail.com'
	}
}
