
node {

def mavenHome = tool name: 'maven3.9.8'

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([cron('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"
echo "The node name is: ${env.NODE_NAME}"
echo "The Job url is: ${env.JOB_URL}"


stage('CheckoutCode'){
git credentialsId: 'fb7de9c4-b229-4f39-8adc-e857819cb47e', url: 'https://github.com/BharathiNarayan/maven-web-application.git'
}

stage('Build'){
sh "$mavenHome/bin/mvn clean package"
}

stage('ExecutesonarqubeReport'){
sh "$mavenHome/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "$mavenHome/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat'){
sshagent(['3435255d-b2c7-4a26-9f67-f2f5d59382a7']) {
sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.36.150:/opt/apache-tomcat-9.0.95/webapps"
} 
}


}
