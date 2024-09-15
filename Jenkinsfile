pipeline {
    agent {
        node {
            label 'linux || java11 || java17'
        }
    }

    environment {
        TEXT_SUCCESS_BUILD = "[#${env.BUILD_NUMBER}] Project Name : ${JOB_NAME} is Success"
        TEXT_FAILURE_BUILD = "[#${env.BUILD_NUMBER}] Project Name : ${JOB_NAME} is Failure"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    for (int i = 0; i < 10; i++) {
                        echo("Script ${i}")
                    }
                }

                echo('Hello Build')
                sh('./mvnw clean compile test-compile')
                echo('Finish Build')
            }
        }

        stage('Test') {
            steps {
                script {
                    def data = [
                        'firstName': 'Lukman',
                        'lastName': 'Nur Hakim'
                    ]
                    writeJSON(file: 'data.json', json: data)
                }

                echo('Hello Test')
                sh('./mvnw test')
                echo('Finish Test')
            }
        }

        stage('Deploy') {
            steps {
                echo('Hello Deploy')
                sleep(5)
                echo('Finish Deploy')
            }
        }
    }

    post {
        always {
            echo 'I will always say hello again'
        }
        success {
            script {
                sendTelegramMessage(env.TEXT_SUCCESS_BUILD)
            }
            echo 'Build succeeded!'
        }
        failure {
            script {
                sendTelegramMessage(env.TEXT_FAILURE_BUILD)
            }
            echo 'Build failed!'
        }
        cleanup {
            echo "Performing cleanup tasks"
        }
    }
}

// Definisikan method untuk mengirim pesan Telegram
def sendTelegramMessage(String message) {
    withCredentials([
        string(credentialsId: 'telegram-token', variable: 'TOKEN'),
        string(credentialsId: 'telegram-chat-id', variable: 'CHAT_ID')
    ]) {
        def telegramUrl = "https://api.telegram.org/bot${TOKEN}/sendMessage"
        def curlCommand = "curl -s -X POST ${telegramUrl} -d chat_id=${CHAT_ID} -d text='${message}'"

        sh(script: curlCommand, returnStatus: true)
    }
}