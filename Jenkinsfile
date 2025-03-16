pipeline {
    agent {
        docker {
            image 'python:3.9'
        }
    }
    stages {
        stage('Setup Environment') {
            steps {
                sh '''
                apt-get update
                apt-get install -y python3 python3-pip
                python3 -m pip install --upgrade pip
                '''
            }
        }
        stage('Build') {
            steps {
                sh 'python3 -m pip install --upgrade pip'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m unittest discover -s sources -p test_*.py'
            }
        }
    }
}
