pipeline {
    agent any

    environment {
        VENV_DIR = 'venv' // Virtual environment directory
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'live', url: 'https://github.com/prasha18/letsfame.git'
            }
        }

        stage('Setup Python') {
            steps {
                sh 'python3 -m venv $VENV_DIR'
                sh './$VENV_DIR/bin/pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh './$VENV_DIR/bin/python -m unittest discover tests'
            }
        }

        stage('Build Package') {
            steps {
                sh 'zip -r build.zip *'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['your-ssh-credentials']) {
                    sh '''
                    scp build.zip ubuntu@43.205.243.141:/python
                    ssh ubuntu@43.205.243.141 "cd /python && unzip -o build.zip"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed!'
        }
    }
}

