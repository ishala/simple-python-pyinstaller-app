node {
    env.PATH = "/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    stage('Check Docker') {
        sh 'docker --version'
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Setup Environment') {
        sh 'python3 --version'
        sh 'pip3 install pytest --break-system-packages' // Install pytest for testing
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
