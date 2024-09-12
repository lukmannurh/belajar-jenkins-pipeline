pipeline {
    agent {
        node {
            label "linux || java11 || java17"
        }
    }
    stages {
        stage ("Hello") {
            steps {
                echo ("Hello Pipeline")
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