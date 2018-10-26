pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                cmd 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging-tomcat-area'
            }
        }
    }
}