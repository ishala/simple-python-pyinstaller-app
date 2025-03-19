node {
    env.PATH = "/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    stage('Check Docker') {
        sh 'docker --version'
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Build') { 
        sh '''
        python3 --version
        python3 -m venv venv  # Membuat virtual environment
        source venv/bin/activate
        '''
    }

    stage('Test') {
        try {
            sh 'python3 -m unittest discover -s sources -p "test_*.py"'
        } catch (Exception e) {
            echo "Tests failed!"
            currentBuild.result = 'FAILURE'
        }
    }

    stage('Cleanup') {
        echo "Cleaning up workspace..."
        deleteDir()
    }
}
