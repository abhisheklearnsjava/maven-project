pipeline{
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '13.59.150.60', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.191.210.153', description: 'Production Server')
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
					    bat "cd"
                        bat 'winscp /command "open sftp://ec2-user@13.59.150.60/ -privatekey=D:/mywork/jenkinHandson/puttygen-key.ppk" "put C:\Users\Abhishek_S30\.jenkins\workspace\fully-automated-aws\webapp\target\webapp.war /var/lib/tomcat7/webapps/" "exit"'
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat 'winscp /command "open sftp://ec2-user@18.191.210.153/ -privatekey=D:/mywork/jenkinHandson/puttygen-key.ppk" "put C:\Users\Abhishek_S30\.jenkins\workspace\fully-automated-aws\webapp\target\webapp.war /var/lib/tomcat7/webapps/" "exit" " "exit"'
                    }
                }
            }
        }
    }
}