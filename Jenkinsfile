node {
    stage('Setup Environment') {
        echo 'Checking and installing Python if necessary...'
        sh '''
        if ! command -v python3 &>/dev/null; then
            echo "Python not found, installing..."
            apt-get update && apt-get install -y python3 python3-pip
        fi
        '''
    }

    stage('Build') {
        echo 'Installing dependencies...'
        sh 'python3 -m pip install --upgrade pip'
    }

    stage('Test') {
        echo 'Running unit tests...'
        sh 'python3 -m unittest discover -s sources -p test_*.py'
    }
}
