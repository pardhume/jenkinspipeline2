node('master')
{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
	
def mavenhome=tool name:"mavan3.8.4"

	stage('checkout')
	{
		git branch: 'development', credentialsId: 'dfd12363-3639-499f-bb85-33e55274af85', url: 'https://github.com/flikparknbkrist/maven-web-application.git'
	}
	stage('build')
	{
	sh "${mavenhome}/bin/mvn clean package"
	}

	stage('excuitesonarqubereport')
	{
	sh "${mavenhome}/bin/mvn clean package sonar:sonar"
	}
	stage('uploadaArtifactorIntoNexus')
	{
	sh "${mavenhome}/bin/mvn clean deploy"
	}
	stage('deployeeapplicationtomcatserver')
	{
sshagent(['3647d27d-7a19-4368-bc80-89ba62046780'])  {
sh"scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.16.216.199://opt/apache-tomcat-9.0.56/webapps/"
    
}
	}

	stage('emailnotification')
	{
	emailext body: '''build is is over

	by
	pardha saradhi
	9030199150''', subject: 'build is over', to: 'pardhume@gmail.com,bheemavaramuma@gmail.com'
	}
}

	
