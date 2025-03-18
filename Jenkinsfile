node {
    env.PATH = "/usr/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    stage('Check Docker') {
        sh 'docker --version'
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Setup Python Virtual Environment') {
        sh '''
        python3 -m venv venv
        source venv/bin/activate
        pip install --upgrade pip
        pip install pytest
        '''
    }

    stage('Run Tests') {
        try {
            sh '''
            source venv/bin/activate
            pytest sources/test_calc.py --disable-warnings
            '''
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
