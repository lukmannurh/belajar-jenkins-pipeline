pipeline {
    agent {
        node {
            label "linux || java11 || java17"
        }
    }

    stages {
        stage ("Build") {
            steps {

                script {
                    for(int i=0; i<10; i++) {
                        echo("Script ${i}")
                    }
                }

                echo ("Hello Build")
                sh("./mvnw clean compile test-compile")
                echo("Finish Build")
            }
        }

        stage ("Test") {
            steps {

                script {
                    def data = [
                        "firstName": "Lukman",
                        "lastName": "Nur Hakim"
                    ]
                    writeJSON("file": "data.json", json: data)
                }

                echo ("Hello Test")
                sh("./mvnw test")
                echo("Finish Test")
            }
        }

        stage ("Deploy") {
            steps {
                echo ("Hello Deploy")
                sleep(5)
                echo("Finish Deploy")
            }
        }
    }

    post {
        always {
            echo "I will always say hello again"
        }
        success {
            echo "yeay, success"
        }
        failure {
            echo "Oh no, failure"
        }
        cleanup {
            echo "Don't care success or error"
        }
    }
}