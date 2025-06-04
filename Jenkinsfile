pipeline {
    agent any

    environment {
        IMAGE_NAME = 'mi-aplicacion'  // Cambia esto si quieres otro nombre
    }

    stages {
        stage('Clonar repositorio') {
            steps {
                git branch: 'main', url: 'https://github.com/TU_USUARIO/TU_REPO.git'
            }
        }

        stage('Obtener último tag') {
            steps {
                script {
                    def tag = sh(script: "git describe --tags \$(git rev-list --tags --max-count=1)", returnStdout: true).trim()
                    env.LAST_TAG = tag
                    echo "Último tag detectado: ${env.LAST_TAG}"
                }
            }
        }

        stage('Construir imagen Docker') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${env.LAST_TAG} ."
            }
        }

        stage('Crear contenedor') {
            steps {
                sh "docker run -d --name ${IMAGE_NAME}-${env.LAST_TAG} ${IMAGE_NAME}:${env.LAST_TAG}"
            }
        }
    }
}
