pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7' // Must match the name in Jenkins "Global Tool Configuration"
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
                    echo "Running Maven build with Checkstyle skipped..."
                    mvn clean package -Dcheckstyle.skip=true -X
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Checking for WAR file in target/"
                    ls -lh target/

                    WAR_FILE=$(ls target/*.war 2>/dev/null || true)
                    if [ -n "$WAR_FILE" ]; then
                        echo "Deploying WAR file to Tomcat..."
                        sudo cp $WAR_FILE /opt/tomcat/webapps/
                        echo "Deployment done."
                    else
                        echo "❌ WAR file not found! Skipping deployment."
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
