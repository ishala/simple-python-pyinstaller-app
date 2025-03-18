pipeline {
    agent any
    environment {
        PATH = "C:\\\\Program Files\\\\Docker\\\\Docker\\\\resources\\\\bin;${env.PATH}"
    }
    stages {
        stage('Build') { 
            steps {
                sh 'docker --version'
            }
        }
    }
}
