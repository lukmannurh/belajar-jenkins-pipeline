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
}