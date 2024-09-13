pipeline {
    agent {
        node {
            label "linux || java11 || java17"
        }
    }

    stages {
        stage ("Build") {
            steps {
                echo ("Hello Build")
                sleep(5)
                echo("Finish Build")
            }
        }

        stage ("Test") {
            steps {
                echo ("Hello Test")
                sleep(5)
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