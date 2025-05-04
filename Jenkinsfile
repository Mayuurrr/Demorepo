pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Mayuurrr/Demorepo.git'
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