node {
    env.PATH = "/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    stage('Check Docker') {
        sh 'docker --version'
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Check and Install Python') {
        sh '''
        if ! command -v python3 &> /dev/null
        then
            echo "Python3 not found, installing..."
            sudo apt-get update
            sudo apt-get install -y python3 python3-pip
        else
            echo "Python3 is already installed"
        fi
        '''
    }

    stage('Setup Environment') {
        sh 'python3 --version'
        sh 'pip3 install --upgrade pip'
        sh 'pip3 install pytest' // Install pytest for testing
    }

    stage('Run Tests') {
        try {
            sh 'pytest sources/test_calc.py --disable-warnings'
        } catch (Exception e) {
            echo "Tests failed!"
            currentBuild.result = 'FAILURE'
        }
    }

    stage('Cleanup') {
        echo "Cleaning up workspace..."
        deleteDir() // Clean up workspace after completion
    }
}
