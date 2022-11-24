node{

def mavenHome = tool name: 'maven3.8.6'

echo "The job name is: ${env.JOB_NAME}"
echo "The Build NUmber is: ${env.BUILD_NUMBER}"
echo "The Node name is: ${env.NODE_NAME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

//Checkout code from Git
stage('CheckoutCode'){
git branch: 'development', credentialsId: 'c9635f5b-e3cd-4ed3-86a0-ad865b5320bc', url: 'https://github.com/devopstrainingNS/maven-web-application.git'
}

//Build
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

//Execute SOnarQube Report
stage('ExecuteSonarQubeReport'){
sh"${mavenHome}/bin/mvn sonar:sonar"
}

//Deploy application into tomcat server
stage('DeployApp'){
sshagent(['2a776880-aed2-43e3-a0ec-2a04a2d9096c']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.45.77:/opt/apache-tomcat-9.0.68/webapps/"
}
}
    
}
