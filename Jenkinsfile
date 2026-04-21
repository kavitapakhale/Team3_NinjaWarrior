pipeline {
    agent any

    environment {
        // Define environment variables here
        PROJECT_NAME = "My-API-Tests"
    }

    stages {
        stage('Initialize') {
            steps {
                echo "Starting ${env.PROJECT_NAME}..."
                sh 'node --version'
                sh 'newman --version'
            }
        }

        stage('Checkout') {
            steps {
                // This checks out code from the repo linked to the Jenkins job
                checkout scm
            }
        }

        stage('Run API Tests') {
            steps {
                // Running Newman against your collection in the repo
                withCredentials([usernamePassword(credentialsId: 'API_PASSWORD', 
                                  usernameVariable: 'API_USER', 
                                  passwordVariable: 'API_PASS')]) {
                     sh "newman run Team3_NinjaWarrior.postman_collection-Latest.json --env-var password=${API_PASS} --env-var username=${API_USER} --env-var baseurl=https://lms-hackathon-api-april-2026-8b55b53c8fab.herokuapp.com/lms --reporters cli,htmlextra --reporter-htmlextra-export reports/report.html"
                 }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/*.html', fingerprint: true
            echo "Pipeline finished."
        }
        failure {
            echo "API tests failed! Check the logs."
        }
    }
}
