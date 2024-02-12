pipeline {
    agent any

    environment {
        REGISTRE = 'solofonore/html'
        DOCKER_IMAGE = "${REGISTRE}:version-${env.BUILD_ID}"
    //  GITHU_TOKEN = credentials('github_token')
        VERSION_FILE = 'version.txt'
        DEFAULT_VERSION = '1'
        VERSION_NUMBER = ''
    }

    stages {
        stage('Clonage du dépôt') {
            steps {
                sh "rm -R jenkins && git clone https://github.com/solofo772/jenkins.git && cd jenkins/"
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
                withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    // Supprimer cette ligne si vous ne voulez pas pousser l'image Docker
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        stage('Cleanup Artifacts'){
            steps {
                script {
                    sh "docker rmi ${DOCKER_IMAGE}"
                }
            }
        }
        stage('Correction du fichier de déploiement') {
            steps {
                script {
                   def manifestPath = "${pwd()}/manifest"
                   sh "cat ${manifestPath}/deployment.yaml"
                   sh "sed -i 's|solofonore/html:version-20|${DOCKER_IMAGE}|g' ${manifestPath}/deployment.yaml"
                   sh "cat ${manifestPath}/deployment.yaml"
                } 
            }
        }


        stage('Push the new change deployment to GIT'){
            steps {
                script {
                    def manifestPath = "${pwd()}/manifest"
                    sh """
                      git config --global user.email "solofonore@gmail.com"
                      git config --global user.name "solofo772"
                      git add ${manifestPath}/deployment.yaml
                      git commit -m "Update Deployment Manifest"
                      git branch
                   
                    """
                    withCredentials([string(credentialsId: 'github_token', variable: 'GITHUB_TOKEN')]) {
                      sh "git push https://$GITHUB_TOKEN@github.com/solofo772/jenkins.git HEAD:main"
                    }    
                }
            }
        }
    }
}

//git push https://${github_token}@github.com/solofo772/jenkins.git
