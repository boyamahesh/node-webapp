pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Git checkout')
        {
        steps{
            git credentialsId: 'nodeapp', url: 'https://github.com/boyamahesh/node-webapp.git'
        }
        }
        stage(' Maven Build'){
            steps{
                sh'mvn clean package'
                sh 'mv target/*.war target/myweb.war'
            }
        }
          
        stage('deploy-dev')
           {
         steps{
             sshagent(['tomcat-new'])
             {
                 steps
           {
      sh """
      scp -o StrictHostKeyChecking-no target/myweb.war  ec2-user@172.31.58.73:home/ec2-user/apache-tomcat-9.0.58/webapps/
      
      ssh ec2-user@172.31.58.73:home/ec2-user/apache-tomcat-9.0.58/bin/shutdown.sh
      
      ssh ec2-user@172.31.58.73:home/ec2-user/apache-tomcat-9.0.58/bin/startup.sh
      
      """
               
           }
        }
     }
   }
 }
