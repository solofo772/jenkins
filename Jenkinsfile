pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-repo')
        DOCKER_IMAGE = 'solofonore/html'
        VERSION_FILE = 'version.txt'
        DEFAULT_VERSION = '1'
    }

    stages {
        stage('Clonage du dépôt') {
            steps {
                git branch: 'main', url: 'https://github.com/solofo772/jenkins.git'
            }
        }

        stage('Récupération de la version') {
            steps {
                script {
                    if (fileExists(VERSION_FILE)) {
                        VERSION_NUMBER = readFile(VERSION_FILE).trim()
                    } else {
                        VERSION_NUMBER = DEFAULT_VERSION
                    }
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
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
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
