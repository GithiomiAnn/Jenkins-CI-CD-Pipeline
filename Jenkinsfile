pipeline {
    agent any

    environment {
        // Environment variables
        MAVEN_HOME = tool name: 'Maven 3.9.9', type: 'maven'
        EMAIL_RECIPIENT = 's223770775@deakin.edu.au'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the code using Maven...'
                bat "\"${MAVEN_HOME}/bin/mvn\" clean package"
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit and Selenium...'
                bat "\"${MAVEN_HOME}/bin/mvn\" test"
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis using SonarQube...'
                // Add your SonarQube analysis command here
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan using OWASP Dependency-Check...'
                // Add your OWASP Dependency-Check command here
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to the staging server on AWS EC2...'
                // Add your deployment command here
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on the staging environment...'
                // Add your integration tests command here
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to the production server on AWS EC2...'
                // Add your production deployment command here
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
