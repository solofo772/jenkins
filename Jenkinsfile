pipeline {
    agent any

    environment {
        DOCKER_USERNAME = env.DOCKER_USERNAME
        DOCKER_PASSWORD = env.DOCKER_PASSWORD
        DOCKER_IMAGE = 'solofonore/html'
        VERSION_FILE = 'version.txt'
    }

    stages {
        stage('Clonage du dépôt') {
            steps {
                git 'https://github.com/solofo772/jenkins.git'
            }
        }

        stage('Récupération de la version') {
            steps {
                script {
                    VERSION_NUMBER = readFile(VERSION_FILE).trim()
                }
            }
        }

        stage('Construction de l\'image Docker') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${VERSION_NUMBER}")
                }
            }
        }

        stage('Authentification Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', [username: DOCKER_USERNAME, password: DOCKER_PASSWORD]) {
                        docker.image("${DOCKER_IMAGE}:${VERSION_NUMBER}").push('latest')
                    }
                }
            }
        }

        stage('Incrémentation de la version') {
            steps {
                script {
                    def newVersion = Integer.parseInt(VERSION_NUMBER) + 1
                    writeFile file: VERSION_FILE, text: "${newVersion}"
                }
            }
        }
    }
}
