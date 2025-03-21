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
        python3 -m unittest --help
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

    stage('Deploy') {
        sh 'docker run --rm -v $PWD:/app -w /app python:3.9 bash -c "\
        pip install pyinstaller && \
        pyinstaller --onefile sources/add2vals.py"'
        
        sleep 60  // Tunggu 1 menit
        
        echo 'Pipeline has finished successfully.'

        archiveArtifacts artifacts: 'dist/add2vals', fingerprint: true
    }
}
