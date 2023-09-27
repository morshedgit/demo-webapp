pipeline {
    agent any

    environment {
        TOMCAT_SERVER = "${env.TOMCAT_SERVER}"
        TOMCAT_PATH = "${env.TOMCAT_PATH}"
        TOMCAT_USER = credentials('tomcat_deploy').username
        TOMCAT_PASS = credentials('tomcat_deploy').password
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
                    curl --upload-file target/demo-app.war "http://${TOMCAT_USER}:${TOMCAT_PASS}@${TOMCAT_SERVER}:8888/manager/text/deploy?path=${TOMCAT_PATH}&update=true"
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
