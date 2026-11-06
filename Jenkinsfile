pipeline {
    agent any

    tools {
        // Matches the exact Name you entered in the Jenkins Tools configuration screen
        nodejs 'NodeJS-24'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Newman & Reporter') {
            steps {
                sh 'npm install newman newman-reporter-htmlextra'
            }
        }

        stage('Run Postman Collection') {
            steps {
                // Simplified with npx to reliably execute local node_modules binaries on macOS
                sh 'npx newman run "Booking API Workflow.json" -r htmlextra --reporter-htmlextra-export api-test-report.html'
            }
        }
    }

    post {
        always {
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'api-test-report.html',
                reportName: 'Newman API Test Report'
            ])
        }
    }
}

