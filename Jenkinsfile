pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-repo')
        REGISTRE = 'solofonore/html'
        DOCKER_IMAGE = "${REGISTRE}:version-${env.BUILD_ID}"
        VERSION_FILE = 'version.txt'
        DEFAULT_VERSION = '1'
        VERSION_NUMBER = ''
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
                    docker.build("${DOCKER_IMAGE}", '.')
                }
            }
        }

        stage('Authentification Docker Hub') {
            steps {
                script {
                    {
                        docker.image("${DOCKER_IMAGE}").run('-p 80:80')
                        docker.ps
                    }
                }
            }
        }
        stage('Affichage des conteneurs en cours d\'exécution') {
            steps {
                script {
                    sh "docker ps"
                }
            }
        }
    }
}
