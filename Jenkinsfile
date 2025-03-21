node {
    stage('Pre-Cleanup') {
        echo "Cleaning workspace before starting..."
        cleanWs()
        sh '''
        docker ps -a | grep python && docker rm -f $(docker ps -aq) || true
        '''
    }

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

    stage('Manual Approval') {
        echo "Waiting for manual approval..."
        try {
            timeout(time: 5, unit: 'MINUTES') {
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        } catch (Exception e) {
            echo "No approval received, stopping pipeline."
            currentBuild.result = 'ABORTED'
            return
        }
    }

    stage('Deploy') {
        echo "Starting application..."
        sh 'docker run -d --name my_app -p 5000:5000 python:3.9 bash -c "\
        pip install pyinstaller && \
        pyinstaller --onefile sources/add2vals.py"'

        echo "Application is running."
        sh 'sleep 60'
        echo "Stopping application..."
        sh 'docker stop my_app && docker rm my_app'
        
    }

    stage('Post-Cleanup') {
        echo "Cleaning up workspace after finishing..."
        cleanWs()
    }
}
