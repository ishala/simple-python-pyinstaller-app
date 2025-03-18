node {
    withEnv(["PATH=/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"]) {
        
        stage('Check Docker') {
            sh 'docker --version'
        }

        stage('Checkout Repository') {
            checkout scm
        }

        stage('Set Up Python Environment') {
            sh 'python3 -m venv venv'
            sh 'source venv/bin/activate && pip install --upgrade pip'
            sh 'source venv/bin/activate && pip install -r requirements.txt || echo "No requirements.txt found, skipping..."'
        }

        stage('Run Python Tests') {
            sh 'source venv/bin/activate && pytest --junitxml=report.xml'
        }
    }
}
