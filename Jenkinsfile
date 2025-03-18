node {
    env.PATH = "/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    stage('Check Docker') {
        sh 'docker --version'
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Setup Environment') {
        sh '''
        python3 --version
        pip3 install --upgrade pip --break-system-packages
        pip3 install pytest --break-system-packages
        '''
    }

    stage('Run Tests') {
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
