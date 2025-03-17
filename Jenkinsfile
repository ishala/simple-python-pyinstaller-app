pipeline {
    agent {
        docker {
            image 'docker:24.0.7-dind'  // Pastikan menggunakan versi terbaru DinD
            args '--privileged'         // Diperlukan untuk menjalankan Docker di dalam Docker
        }
    }
    stages {
        stage('Setup Docker') {
            steps {
                sh 'dockerd-entrypoint.sh & sleep 5'  // Jalankan daemon Docker dalam container
                sh 'docker version'  // Verifikasi Docker tersedia
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t my-python-app .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm my-python-app python -m unittest discover -s sources -p test_*.py'
            }
        }
    }
}
