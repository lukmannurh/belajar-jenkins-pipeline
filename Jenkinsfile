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
        choice(name: 'SOCIAL_MEDIA', choices: ['Instagram', 'Facebook', 'Twitter'], description: 'Which social media?')
        password(name: 'SECRET', defaultValue: '', description: 'Encrypt key')
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('OS Setup') {
            steps {
                script {
                    osSetup()
                }
            }
        }

        stage('Preparation') {
            steps {
                script {
                    parallelPreparation()
                }
            }
        }

        stage('Parameter') {
            steps {
                script {
                    printParameters()
                }
            }
        }

        stage('Prepare') {
            environment {
                APP = credentials('lukman_rahasia')
            }
            steps {
                script {
                    printEnvironmentInfo()
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    printScripts()
                }
                echo 'Hello Build'
                sh './mvnw clean compile test-compile'
                echo 'Finish Build'
            }
        }

        stage('Test') {
            steps {
                script {
                    writeJSONFile()
                }
                echo 'Hello Test'
                sh './mvnw test'
                echo 'Finish Test'
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY }
            }
            input {
                message 'Can we deploy?'
                ok 'Yes, of course'
                submitter 'Lukman, Luke'
                parameters {
                    choice(name: 'TARGET_ENV', choices: ['DEV', 'QA', 'PROD'], description: 'Which environment')
                }
            }
            steps {
                echo "Deploy to ${TARGET_ENV}"
            }
        }

        stage('Release') {
            when {
                expression { params.DEPLOY }
            }
            steps {
                echo 'Release it'
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

// Helper functions
def osSetup() {
    def osConfigurations = [
        'linux': ['32', '64'],
        'windows': ['32', '64'],
        'mac': ['64']
    ]

    osConfigurations.each { os, arcs ->
        arcs.each { arc ->
            if (!(os == 'mac' && arc == '32')) {
                echo "SETUP ${os} ${arc}"
                // Add your setup logic here
            }
        }
    }
}

def parallelPreparation() {
    parallel(
        'Prepare Java': {
            echo 'Prepare Java'
            sleep(5)
        },
        'Prepare Maven': {
            echo 'Prepare Maven'
            sleep(5)
        }
    )
}

def printParameters() {
    echo "Hello ${params.NAME}!"
    echo "Your description is ${params.DESCRIPTION}"
    echo "Your social media is ${params.SOCIAL_MEDIA}"
    echo "Need to deploy: ${params.DEPLOY}"
    echo "Your secret is ${params.SECRET}!"
}

def printEnvironmentInfo() {
    echo "Author: ${AUTHOR}"
    echo "Start Job: ${env.JOB_NAME}"
    echo "Start Build: ${env.BUILD_NUMBER}"
    echo "Branch Name: ${env.BRANCH_NAME}"
    echo "APP Username: ${APP_USR}"
    sh 'echo "APP Password: $APP_PSW" > "rahasia.txt"'
}

def writeJSONFile() {
    def data = [
        'firstName': 'Lukman',
        'lastName': 'Nur Hakim'
    ]
    writeJSON(file: 'data.json', json: data)
}

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
