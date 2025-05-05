pipeline {
    agent any

    environment {
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
                cleanWs()
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Deploying WAR file to Tomcat..."
                    if ls target/*.war 1> /dev/null 2>&1; then
                        cp target/*.war /opt/tomcat/webapps/
                    else
                        echo "WAR file not found!"
                        exit 1
                    fi
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
