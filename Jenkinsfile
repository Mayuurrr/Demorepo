pipeline {
    agent any

    tools {
        maven 'Maven 3' // Make sure this is configured in Global Tool Configuration
    }

    stages {
        stage('Clone') {
            steps {
                git credentialsId: 'github-pat', url: 'https://github.com/Mayuurrr/Demorepo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh 'cp target/*.war /opt/tomcat/webapps/'
            }
        }
    }
}
