pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '3.88.31.142', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '54.210.167.152', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                        sh "scp -i /home/akash/Desktop/DevOps/JenkinsKeypair.pem **/target/*.war ec2-user@${params.tomcat_dev}:/opt/apache-tomcat-8.5.54/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/akash/Desktop/DevOps/JenkinsKeypair.pem **/target/*.war ec2-user@${params.tomcat_prod}:/opt/apache-tomcat-8.5.54/webapps"
                    }
                }
            }
        }
    }
}