pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3032:3000'
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
                    def userInput = input(
                        id: 'userInput', message: 'Lanjutkan ke tahap Deploy?',
                        parameters: [
                            [$class: 'BooleanParameterDefinition', defaultValue: true, description: 'Proceed to Deploy', name: 'Proceed']
                        ]
                    )

                    if (!userInput.Proceed) {
                        error 'Pipeline execution aborted by user'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh './jenkins/scripts/deliver.sh'
                    sleep(time: 60, unit: 'SECONDS') // Menjeda eksekusi selama 1 menit
                    sh './jenkins/scripts/kill.sh'
                }
            }
        }
    }
}
