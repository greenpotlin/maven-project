pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                bat set PATH=%BASEPATH%
				bat set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_161
				bat set MAVEN_HOME=C:\Users\khasnsa\Downloads\apache-maven-3.5.2
				bat set PATH=%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%PATH%

				bat 'mvn clean package'
                
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}