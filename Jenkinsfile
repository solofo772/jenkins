pipeline {
    agent any

    stages {
        stage("Build") {
            steps {
                sh 'echo "Building the application"'
            }
        }

        stage("Test") {
            steps {
                sh 'echo "Testing the application"'
            }
        }

        stage("Deploy") {
            steps {
                sh 'echo "Deploying on production"'
            }
        }
    }
}
