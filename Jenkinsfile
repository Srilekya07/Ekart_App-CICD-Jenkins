pipeline {
    agent any

    environment {
        DEV_SERVER  = "deploy@DEV_IP"
        TEST_SERVER = "deploy@TEST_IP"
        PROD_SERVER = "deploy@PROD_IP"
        WAR_NAME    = "app.war"
        TOMCAT_DIR  = "/var/lib/tomcat9/webapps"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/Srilekya07/Ekart_App-CICD-Jenkins.git', branch: 'main'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh """
                scp target/${WAR_NAME} ${DEV_SERVER}:${TOMCAT_DIR}
                ssh ${DEV_SERVER} 'sudo systemctl restart tomcat9'
                """
            }
        }

        stage('Deploy to Test') {
            steps {
                sh """
                scp target/${WAR_NAME} ${TEST_SERVER}:${TOMCAT_DIR}
                ssh ${TEST_SERVER} 'sudo systemctl restart tomcat9'
                """
            }
        }

        stage('Deploy to Prod') {
            steps {
                sh """
                scp target/${WAR_NAME} ${PROD_SERVER}:${TOMCAT_DIR}
                ssh ${PROD_SERVER} 'sudo systemctl restart tomcat9'
                """
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully"
        }
        failure {
            echo "Deployment failed. Check console output."
        }
    }
}
