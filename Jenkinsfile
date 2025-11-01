pipeline {
    agent any

    stages {
        stage('Install dependencies') {
            steps {
                    sh '''
                        # 1. Update package list and install Python and venv (No sudo needed)
                        apt-get update
                        apt-get install -y python3 python3-venv

                        # 2. Create the virtual environment
                        python3 -m venv venv

                        # 3. Install project dependencies into the venv
                        ./venv/bin/pip install -r requirements.txt
                    '''
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