pipeline {
    agent any
    environment {
        PATH = "/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    }
    stages {
        stage('Check Docker') {
            steps {
                sh 'docker --version'
            }
        }
    }
}
