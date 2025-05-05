pipeline {
    agent any

    environment {
        PATH = "/usr/share/maven/bin:$PATH"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone') {
            steps {
                git url: 'https://github.com/Mayuurrr/Demorepo.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Running Maven build..."
                    mvn clean package -Dcheckstyle.skip=true -X
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Checking for WAR file in target/..."
                    ls -lh target/

                    WAR_FILE=$(find target -name "*.war" | head -n 1)
                    if [ -n "$WAR_FILE" ]; then
                        echo "Deploying WAR file to Tomcat..."
                        sudo cp "$WAR_FILE" /opt/tomcat/webapps/
                        echo "✅ Deployed: $WAR_FILE"
                    else
                        echo "❌ WAR file not found!"
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
            archiveArtifacts artifacts: 'target/*.war', fingerprint: true
        }
        failure {
            echo '❌ Pipeline failed. Please check logs.'
        }
    }
}
