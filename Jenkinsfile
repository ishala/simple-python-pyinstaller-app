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

    stage('Deploy') {
        echo "Starting application..."
        sh 'docker run -d --name my_app -p 5000:5000 python:3.9 bash -c "\
        pip install pyinstaller && \
        pyinstaller --onefile sources/add2vals.py"'

        echo "Application is running."

        // 🕒 Menunggu input manual, jika tidak ada interaksi dalam 1 menit, lanjut otomatis
        def userInput = null
        try {
            timeout(time: 1, unit: 'MINUTES') {
                userInput = input message: 'Coba aplikasi sekarang! Klik "Proceed" untuk lanjut atau tunggu 1 menit untuk otomatis berakhir.'
            }
        } catch (Exception e) {
            echo "No user input detected, proceeding automatically..."
        }

        // Jika user tidak memberikan input, tetap tunggu 1 menit sebelum lanjut
        // if (userInput == null) {
        //     echo "Waiting for 1 minute before proceeding..."
        //     sh 'sleep 60'
        // }

        echo "Stopping application..."
        sh 'docker stop my_app && docker rm my_app'

        echo "Deploying to Railway..."

        // Pastikan sudo sudah terinstall di jenkins-docker
        // sh 'apt-get update && apt-get install -y sudo'
        sh 'docker ps'

        // Install Railway CLI di dalam jenkins-docker
        sh 'sudo curl -fsSL https://railway.app/install.sh | sh'

        withCredentials([string(credentialsId: 'RAILWAY_API_TOKEN', variable: 'RAILWAY_TOKEN')]) {
            sh 'sudo railway login --token $RAILWAY_API_TOKEN'
        }


        sh '''
        railway init --service submission-cicd-pipeline
        railway up
        '''

        echo 'Pipeline has finished successfully.'
    }

    stage('Post-Cleanup') {
        echo "Cleaning up workspace after finishing..."
        cleanWs()
    }
}
