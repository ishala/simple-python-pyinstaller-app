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

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                sh 'python3 --version'
                sh 'pip3 install --upgrade pip'
                sh 'pip3 install pytest' // Install pytest untuk testing
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh 'pytest sources/test_calc.py --disable-warnings'
                    } catch (Exception e) {
                        echo "Tests failed!"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up workspace..."
                deleteDir() // Bersihkan workspace setelah selesai
            }
        }
    }
}
