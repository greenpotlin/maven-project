pipeline {
    agent any

    parameters {
        string(name: 'tomcat_dev', defaultValue: '54.165.137.243', description: 'Staging dev server')
        string(name: 'tomcat_prod', defaultValue: '52.205.250.218', description: 'prod server')
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
                            bat "scp -i C:/Users/khasnsa/Desktop/AWS key/amazon-key.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                        }
                    }

                    stage ("Deploy to Production"){
                        steps {
                            bat "scp -i C:/Users/khasnsa/Desktop/AWS key/amazon-key.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        }
                    }
                }
            }
        }


}

