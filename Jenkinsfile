pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                batchFile 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
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