pipeline {
    agent any

    stages {
        stage('Install dependencies') {
            steps {
                sh 'python3 -m venv venv'
                sh './venv/bin/pip install -r requirements.txt'
            }
        }

        stage('Run tests') {
            steps {
                sh './venv/bin/pytest --junitxml=report.xml'
            }
        }

        stage('Publish Report') {
            steps {
                junit 'report.xml'
            }
        }
    }
    post {
        // This 'always' block runs regardless of the build result
        always {
            // Send a success/failure message
            slackSend(
                channel: '#builds', // e.g., '#devops-alerts'
                color: 'good',                 // default color if successful
                message: "Build ${currentBuild.fullDisplayName} completed. Status: ${currentBuild.currentResult}",
                // credentialsId is optional if you configure the global settings
            )
        }
        
        // This 'failure' block overrides the 'always' color if the build fails
        failure {
            slackSend(
                channel: '#builds',
                color: 'danger', // Red for failure
                message: "🔴 FAILURE: Build ${currentBuild.fullDisplayName} failed! Check console output: ${env.BUILD_URL}"
            )
        }
    }
}