pipeline {
    agent any

    environment {
        REGISTRE = 'solofonore/html'
        DOCKER_IMAGE = "${REGISTRE}:version-${env.BUILD_ID}"
        VERSION_FILE = 'version.txt'
        DEFAULT_VERSION = '1'
        VERSION_NUMBER = ''
    }

    stages {
        stage('Clonage du dépôt') {
            steps {
                sh "rm -R jenkins"
                sh "git clone https://github.com/solofo772/jenkins.git"
                sh "cd jenkins/"
            }
        }

        stage('Récupération de la version') {
            steps {
                sh '''
                    if [ -f "${VERSION_FILE}" ]; then
                        VERSION_NUMBER=$(cat "${VERSION_FILE}" | tr -d '[:space:]')
                    else
                        VERSION_NUMBER="${DEFAULT_VERSION}"
                    fi
                '''
            }
        }

     stage('Construction de l\'image Docker') {
        steps {
           withCredentials([usernamePassword(credentialsID: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
            sh "docker build -t ${DOCKER_IMAGE} ."
            sh "echo $PASS | docker login -u $USER --password-stdin"
            sh "docker push ${DOCKER_IMAGE}"
                                          }
             }
    }

    }
}
