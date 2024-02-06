pipeline {
    agent any

    stages {
        stage("Init") {
            steps {
                script {
                    // Charger le script externe
                    def gv = load "script.groovy"
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    // Appeler la fonction buildApp() chargée depuis script.groovy
                    gv.buildApp()
                }
            }
        }

        stage("Test") {
            steps {
                script {
                    // Appeler la fonction testApp() chargée depuis script.groovy
                    gv.testApp()
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    // Appeler la fonction deployApp() chargée depuis script.groovy
                    gv.deployApp()
                }
            }
        }
    }
}
