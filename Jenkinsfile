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
                git url: 'https://github.com/ORG/REPO.git'
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
        failure {
            emailext(
                subject: "❌ Jenkins Pipeline Failed: ${env.JOB_NAME}",
                body: """
                Job: ${env.JOB_NAME}
                Build: ${env.BUILD_NUMBER}
                Stage: ${env.STAGE_NAME}
                Check Jenkins console logs.
                """,
                to: "yourmail@example.com"
            )
        }

        success {
            emailext(
                subject: "✅ Deployment Successful",
                body: "Application deployed successfully to all servers.",
                to: "yourmail@example.com"
            )
        }
    }
}
