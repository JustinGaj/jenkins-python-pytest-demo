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
        always {
            slackSend(
                channel: '#builds',
                color: 'good',
                message: "Build ${currentBuild.fullDisplayName} completed. Status: ${currentBuild.currentResult}",
            )
        }
        
        failure {
            slackSend(
                channel: '#builds',
                color: 'danger',
                message: "🔴 FAILURE: Build ${currentBuild.fullDisplayName} failed! Check console output: ${env.BUILD_URL}"
            )
        }
    }
}