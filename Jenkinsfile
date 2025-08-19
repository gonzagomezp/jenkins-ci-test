pipeline {

    agent any

    // Definir la versión y otras variables de entorno aquí
    environment {
        VERSION = 'v1.0'
    }

    tools {
        // nodejs "NodeJS_7.8"
        nodejs "lts"
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('checkout') {
            steps {
                script {
                    // Para acceder a la variable en un script block, usa env.VERSION
                    echo "node${env.BRANCH_NAME}:${env.VERSION}"
                }
            }
        }

        stage('test') {
            steps {
                bat 'npm config set strict-ssl false'
                bat 'npm install'
                bat 'npm test'
            }
        }

        stage('build') {
            steps {
                bat 'npm install'
                bat 'npm run build'
            }
        }

        stage('build docker image') {
            steps {
                // En comandos bat/sh, puedes usar %VARIABLE% en Windows o $VARIABLE en Linux
                bat "docker build -t node${env.BRANCH_NAME}:${VERSION} ."
            }
        }

        stage('push image') {
            steps {
                // El 'credentialsId' debe ser el mismo ID que pusiste en Jenkins
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        // Iniciar sesión en Docker Hub usando las credenciales inyectadas
                        // El --password-stdin es más seguro que pasar la contraseña directamente
                        bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'

                        bat "docker tag node${env.BRANCH_NAME}:${VERSION} gonzagomezp1/node${env.BRANCH_NAME}:${VERSION}"
                        bat "docker push gonzagomezp1/node${env.BRANCH_NAME}:${VERSION}"
                    }
                }
            }
        }
    }
}