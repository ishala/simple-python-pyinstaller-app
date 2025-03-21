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
        sh './jenkins/scripts/deploy.sh'
        input message: 'Apakah aplikasi sudah selesai dijalankan? (Klik "Proceed" untuk menghentikan)'
        sh './jenkins/scripts/stop.sh'
    }


    stage('Cleanup') {
        echo "Cleaning up workspace..."
        deleteDir()
    }
}
