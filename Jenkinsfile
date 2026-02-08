pipeline {
    agent any

    environment {
        DEV_SERVER  = "deploy@3.91.83.50"
        TEST_SERVER = "deploy@34.228.75.209"
        PROD_SERVER = "deploy@3.92.56.250"
        WAR_NAME    = "app.war"
        TOMCAT_DIR  = "/var/lib/tomcat10/webapps"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/Srilekya07/Ekart_App-CICD-Jenkins.git', branch: 'main'
            }
        }

        stage('Build WAR') {
            steps {
                sh '''
                mvn clean package -DskipTests
                cp target/*.war target/app.war
                '''
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh """
                scp target/${WAR_NAME} ${DEV_SERVER}:${TOMCAT_DIR}
                ssh ${DEV_SERVER} 'sudo systemctl restart tomcat10'
                """
            }
        }

        stage('Deploy to Test') {
            steps {
                sh """
                scp target/${WAR_NAME} ${TEST_SERVER}:${TOMCAT_DIR}
                ssh ${TEST_SERVER} 'sudo systemctl restart tomcat10'
                """
            }
        }

        stage('Deploy to Prod') {
            steps {
                sh """
                scp target/${WAR_NAME} ${PROD_SERVER}:${TOMCAT_DIR}
                ssh ${PROD_SERVER} 'sudo systemctl restart tomcat10'
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
