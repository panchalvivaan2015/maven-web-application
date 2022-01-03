node
{

def mavenHome = tool name: "maven3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])

stage ('Get the code from GitHub')
{
git branch: 'development', credentialsId: '0b871879-7365-4465-8361-3dbcec0895a2', url: 'https://github.com/panchalvivaan2015/maven-web-application.git'
}

stage ('Build the package')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage ('Executing the SonarQube report')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage ('Upload the artifact into Nexus repository')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage ('Deploy the application into tomcat server')
{
sshagent(['312aa290-a386-4c2a-9989-0f8f22d3aaee']) {
sh 'scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.238.48:/opt/apache-tomcat-9.0.56/webapps/'
}
}

stage ('Send email notification')
{
emailext body: '''Build Over......

Thanks
Santosh Panchal''', subject: 'Build Over........', to: 'panchal.vivaan2015@gmail.com'

}

}
