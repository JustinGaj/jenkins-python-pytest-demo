pipeline {
    agent any

    stages {
        stage('Install dependencies') {
            agent {
                docker {
                    image 'python:3.10-slim' // Use a lightweight Python image
                    // Optional: If you need to access the Jenkins workspace from the container
                    // args '-u root:root' 
                }
            }
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
}