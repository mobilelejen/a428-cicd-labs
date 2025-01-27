pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            steps {
                sh "chmod +x -R ${env.WORKSPACE}"
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan atau "Abort" untuk menghentikan pipeline)'
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                echo 'Aplikasi berjalan selama 1 menit sebelum otomatis dihentikan...'
                sleep time: 1, unit: 'MINUTES'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}