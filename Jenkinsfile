pipeline {
    agent any

    environment {
        // Optional: Set JAVA_HOME and MAVEN_HOME if needed
        // JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        // MAVEN_HOME = '/usr/share/maven'
        PATH = "/usr/share/maven/bin:$PATH"
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/Mayuurrr/Demorepo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Deploying WAR file to Tomcat..."
                    cp target/*.war /opt/tomcat/webapps/
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
