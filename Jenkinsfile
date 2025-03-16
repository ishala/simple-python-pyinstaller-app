pipeline {
    agent {
        docker {
            image 'python:3.9'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'python -m pip install --upgrade pip'
            }
        }
        stage('Test') {
            steps {
                sh 'python -m unittest discover -s sources -p test_*.py'
            }
        }
    }
}