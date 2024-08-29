pipeline {
    agent any

    environment {
        // Environment variables
        MAVEN_HOME = tool name: 'Maven 3.9.9', type: 'maven'
        JAVA_HOME = tool name: 'JDK 22.0.2', type: 'jdk'
        EMAIL_RECIPIENT = 's223770775@deakin.edu.au'
        PATH = "${MAVEN_HOME}/bin:${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the code using Maven...'
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit and Selenium...'
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis using SonarQube...'
                // Add SonarQube analysis steps here
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan using OWASP Dependency-Check...'
                // Add OWASP Dependency-Check steps here
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to the staging server on AWS EC2...'
                // Add deployment steps here
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on the staging environment...'
                // Add integration testing steps here
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to the production server on AWS EC2...'
                // Add production deployment steps here
            }
        }
    }

    post {
        success {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Pipeline Success: ${currentBuild.fullDisplayName}",
                body: "The Jenkins pipeline has completed successfully.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}",
                attachLog: true
            )
        }
        failure {
            emailext(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Pipeline Failure: ${currentBuild.fullDisplayName}",
                body: "The Jenkins pipeline has failed.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nCheck the attached logs for more details.",
                attachLog: true
            )
        }
        always {
            script {
                if (currentBuild.result == 'FAILURE' || currentBuild.result == 'SUCCESS') {
                    echo "Sending notification email to ${EMAIL_RECIPIENT}."
                }
            }
        }
    }
}
