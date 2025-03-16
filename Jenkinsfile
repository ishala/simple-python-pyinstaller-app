pipeline {
    agent {
        docker {
            image 'python:3.9'  // Gunakan image Python
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ishala/simple-python-pyinstaller-app.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'python -m unittest discover -s sources -p test_*.py'
            }
        }
    }
}
