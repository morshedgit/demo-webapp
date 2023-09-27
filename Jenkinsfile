pipeline {
    agent {
        label 'agent1'
    }

    environment {
        TOMCAT_SERVER = "${env.TOMCAT_SERVER}"
        TOMCAT_PATH = "${env.TOMCAT_PATH}"        
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
                sh """
                    curl --upload-file target/demo-app.war "http://${TOMCAT_CREDENTIALS_USR}:${TOMCAT_CREDENTIALS_PSW}@${TOMCAT_SERVER}:8888/manager/text/deploy?path=${TOMCAT_PATH}&update=true"
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
