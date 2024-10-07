pipeline {
    agent any

    environment {
        // Environment variables
        MAVEN_HOME = 'C:\\Program Files\\apache-maven-3.9.9'
        JAVA_HOME = 'C:\\Program Files\\jdk-22.0.2'
        PATH = "${JAVA_HOME}\\bin;${MAVEN_HOME}\\bin;${env.PATH}"
        EMAIL_RECIPIENT = 'annegithiomi1@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the code using Maven...'
                bat 'mvn clean package > build.log'  // Redirect output to build.log
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests using JUnit and Selenium...'
                bat 'mvn test >> build.log'  // Append output to build.log
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis using SonarQube...'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Performing security scan using OWASP Dependency-Check...'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to the staging server on AWS EC2...'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on the staging environment...'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to the production server on AWS EC2...'
            }
        }
    }

    post {
        always {
            script {
                // Archive the build log
                archiveArtifacts artifacts: 'build.log', allowEmptyArchive: true
            }
        }

        success {
            emailext(
            mail(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Pipeline Success: ${currentBuild.fullDisplayName}",
                body: "The Jenkins pipeline has completed successfully.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}",
                attachmentsPattern: "build.log"  // Attach the build log
            )
        }

        failure {
            emailext(
            mail(
                to: "${EMAIL_RECIPIENT}",
                subject: "Jenkins Pipeline Failure: ${currentBuild.fullDisplayName}",
                body: "The Jenkins pipeline has failed.\n\nJob: ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nCheck the attached logs for more details.",
                attachmentsPattern: "build.log"  // Attach the build log
            )
        }
    }
}
