pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.210.171.14', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.233.156.72', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                echo 'Building'
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
                        sh "ssh -i /home/user/Downloads/pipelinetest.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
