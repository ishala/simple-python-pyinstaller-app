node {
    stage('Checkout') {
        checkout scm
    }

    stage('Test') {
        echo 'Running unit tests...'
        sh 'python -m unittest discover -s sources -p test_*.py'
    }
}
