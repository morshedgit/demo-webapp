pipeline {
    agent {
        label 'agent1'
    }

    environment {
        TOMCAT_SERVER = "${env.TOMCAT_SERVER}"
        TOMCAT_PATH = "/usr/local/tomcat/webapps" // This is the default path for Tomcat webapps in the Docker container
        TOMCAT_CREDENTIALS = credentials('tomcat_deploy')
    }

    stages {
        stage('Checkout') {
            steps {
                // If you're using SCM, this step will automatically checkout your code.
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Use scp to copy the war file to the Tomcat webapps directory
                sh """
                    scp -o StrictHostKeyChecking=no target/demo-app.war ${TOMCAT_CREDENTIALS_USR}:${TOMCAT_CREDENTIALS_PSW}@${TOMCAT_SERVER}:${TOMCAT_PATH}/demo-app.war
                """
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
