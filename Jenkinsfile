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
                script {
                    sh 'npm install'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh './jenkins/scripts/test.sh'
                }
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    input message: 'Apakah Anda ingin melanjutkan ke tahap Deploy?',
                        submitter: 'admin',
                        parameters: [booleanParam(defaultValue: true, description: 'Proceed to Deploy', name: 'Proceed')]
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh './jenkins/scripts/deliver.sh'
                    sleep(time: 60, unit: 'SECONDS') // Menjeda eksekusi selama 1 menit
                    input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
                    sh './jenkins/scripts/kill.sh'
                }
            }
        }
    }
}
