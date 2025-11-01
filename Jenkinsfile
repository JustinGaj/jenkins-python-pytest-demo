pipeline {
    agent any

    stages {
        stage('Install dependencies') {
            steps {
                sh 'apt-get update'
                sh 'apt-get install -y python3 python3-venv'
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