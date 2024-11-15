node{

def mavenHome = tool name: 'maven3.9.9'

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([cron('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"
echo "The node name is: ${env.NODE_NAME}"
echo "The Job url is: ${env.JOB_URL}"

stage('CheckoutCode'){
git branch: 'development', credentialsId: 'ed919c56-9299-4da5-aa6e-bb28a26d9ab9', url: 'https://github.com/kallubhai-1179797/maven-web-application.git'
}

stage('Build'){
sh "$mavenHome/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "$mavenHome/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsintonexus'){
sh "$mavenHome/bin/mvn clean deploy"
}

stage('DeployAppintoTomcat'){
sshagent(['03d05b72-4e14-4d9b-bdcf-e119141b8e84']){
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@ip-172-31-42-10:/opt/apache-tomcat-9.0.96/webapps"
}
}

}
