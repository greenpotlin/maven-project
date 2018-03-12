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
                            bat "winscp"
                            bat "open ec2user@${params.tomcat_prod}"
                        }
                    }

                    stage ("Deploy to Production"){
                        steps {
                            bat "winscp -i /home/jenkins/amazon-key.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                        }
                    }
                }
            }
        }


}

