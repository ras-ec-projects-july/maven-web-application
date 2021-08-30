node{
    def mavenHome = tool name: "maven3.8.2"
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
 stage('checkoutcode')
 {
 git branch: 'development', credentialsId: 'cbc0b569-f2da-41e9-a0a2-ddff2457b141', url: 'https://github.com/ras-ec-projects-july/maven-web-application.git'
 }
 
 stage('build')
 {
sh "${mavenHome}/bin/mvn clean package"
}
stage('sonarqubereport')
{
sh "${mavenHome}/bin/mvn clean sonar:sonar"

}
stage('uploadartifactintonexus')
{
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('deployappintomcatserver')
{
sshagent(['358a89c8-36c0-4f0e-9d99-68e3463fb519']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.86.219:/opt/apache-tomcat-9.0.52/webapps/"

}
}
stage('sendemailnotification')
{
emailext body: '''Build is over!!

Regards,
rasool ahmed
8095824967
''', subject: 'Build over!!', to: 'rasool0841@gmail.com'
}


}
