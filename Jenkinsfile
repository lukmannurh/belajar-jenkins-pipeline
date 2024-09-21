pipeline {
    agent {
        node {
            label 'linux || java11 || java17'
        }
    }

    environment {
        AUTHOR = 'Lukman Nur Hakim'
        TEXT_SUCCESS_BUILD = "[#${env.BUILD_NUMBER}] Project Name : ${JOB_NAME} is Success"
        TEXT_FAILURE_BUILD = "[#${env.BUILD_NUMBER}] Project Name : ${JOB_NAME} is Failure"
    }

    parameters {
        string(name: 'NAME', defaultValue: 'GUEST', description: 'What is your name?')
        text(name: 'DESCRIPTION', defaultValue: '', description: 'Tell me about you?')
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Need to deploy?')
        choice(name: 'SOSIAL_MEDIA', choices: ['Instagram', 'Facebook', 'Twitter'], description: 'Which sosial media?')
        password(name: 'SECRET', defaultValue: '', description: 'Encrypt key')
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit : 'MINUTES')
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Preparation') {
            stages{
                stage("Prepare Java") {
                    steps {
                        echo("Prepare Java")
                    }
                }
                stage("Prepare Maven") {
                    steps {
                        echo("Prepare Maven")
                    }
                }
            }
        }


        stage('Parameter'){
            steps{
                echo"Hello ${params.NAME}!"
                echo"You description is ${params.DESCRIPTION}"
                echo"Your sosial media is ${params.SOSIAL_MEDIA}"
                echo"Need to deploy : ${params.DEPLOY}"
                echo"Your secret is ${params.SECRET}!"
            }
        }
        stage('Prepare') {
            environment {
                APP =  credentials('lukman_rahasia')
            }
            steps {
                echo("Author : ${AUTHOR}")
                echo("Start Job : ${env.JOB_NAME}")
                echo("Start Build : ${env.BUILD_NUMBER}")
                echo("Branch Name : ${env.BRANCH_NAME}")
                echo("APP Username : ${APP_USR}")
                sh('echo "APP Password : $APP_PSW" > "rahasia.txt"')
            }
        }

        stage('Build') {
            steps {
                script {
                    printScripts()
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
            input {
                message "Can we deploy?"
                ok "Yes, of course"
                submitter "Lukman, Luke"
                parameters {
                    choice(name: "TARGET_ENV", choices: ['DEV', 'QA', 'PROD'], description: "Whict environment")
                }
            }
            steps {
                echo("Deploy to ${TARGET_ENV}")
            }
        }

        stage('Release') {
            when {
                expression {
                    return params.DEPLOY
                }
            }
            steps {
                echo("Release it")
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
        }
        failure {
            script {
                sendTelegramMessage(env.TEXT_FAILURE_BUILD)
            }
        }
        cleanup {
            echo 'Performing cleanup tasks'
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

def printScripts() {
    (0..9).each { i ->
        echo "Script ${i}"
    }
}
