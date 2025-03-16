node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        echo 'Installing dependencies...'
        sh 'pip install -r requirements.txt || echo "No dependencies to install"'
    }

    stage('Test') {
        echo 'Running unit tests...'
        sh 'python -m unittest discover -s sources -p "test_*.py"'
    }

    stage('Package') {
        echo 'Packaging application...'
        sh 'tar -czvf app.tar.gz sources'
    }

    stage('Deploy') {
        echo 'Deploying application...'
        sh 'mv app.tar.gz /deployment/path || echo "Deployment directory not set"'
    }
}
