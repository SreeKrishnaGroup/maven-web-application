node {
   
      def  MAVEN_HOME ="/opt/maven3.6.3"
      
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3')), pipelineTriggers([pollSCM('* * * * *')])])

    stage('CheckoutCode') {
        git branch: 'development', credentialsId: '0852adfa-a5ad-4954-91c7-0ecfe198b6c5', url: 'https://github.com/SreeKrishnaGroup/maven-web-application.git'
    }
    
    stage('Build'){
        echo "$MAVEN_HOME"
        sh "$MAVEN_HOME/bin/mvn clean package"
    }
    
    stage('ExecuteSonarQubeReport'){
        
        sh "$MAVEN_HOME/bin/mvn sonar:sonar"
    }
    
    stage('uploadArtifactToNexus'){
        
        sh "$MAVEN_HOME/bin/mvn deploy"
    }
    
    stage('DeployAppToTomcat'){
        sshagent(['bf352a21-1109-4473-be19-2bfd91b62dcc']) {
             sh   "scp -o StrictHostKeyChecking=no  target/maven-web-application.war  ec2-user@15.206.167.65:/opt/tomcat9/webapps/"
         }
    }
    
    stage('SendEmailNotification'){
        emailext body: '''Build Over !

           Regards,
           Sree''', subject: 'Build Over', to: 'sreepaadam94@gmail.com'
    }
}
