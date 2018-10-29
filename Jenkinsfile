pipeline{
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.223.162.196', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.223.162.196', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }
 
stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
 
        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "winscp **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps privatekey=D:/mywork/jenkinHandson/tomcat-demo.pem"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "winscp **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps privatekey=D:/mywork/jenkinHandson/tomcat-demo.pem"
                    }
                }
            }
        }
    }
}