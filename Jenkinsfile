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
                    echo "Creating target directory..."
                    mkdir -p target

                    echo "Running Maven build..."
                    mvn clean package -X
                    # If you need to skip checkstyle:
                    # mvn clean package -Dcheckstyle.skip=true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Checking for WAR file..."
                    WAR_FILE=$(ls target/*.war 2>/dev/null || true)
                    if [ -n "$WAR_FILE" ]; then
                        echo "Deploying WAR file to Tomcat..."
                        cp $WAR_FILE /opt/tomcat/webapps/
                    else
                        echo "WAR file not found! Skipping deployment."
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check logs.'
        }
    }
}
