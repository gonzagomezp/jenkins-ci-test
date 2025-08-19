pipeline {

    // Definir la versión como variable
    def VERSION = 'v1.1'

    agent any              // where to run the pipeline (any available node)

    tools {
        // nodejs "NodeJS_7.8"
        nodejs "lts"
    }

    triggers {
        pollSCM('* * * * *') // cad 1 minuto se fij si hy cambios en las ramas?
    }

    stages {
        stage('checkout') {
            steps {
                script {
                    echo "node${env.BRANCH_NAME}:${VERSION}"
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
                        
                        // Ahora los comandos de tag y push funcionarán
                        bat "docker tag node${env.BRANCH_NAME}:${VERSION} gonzagomezp1/node${env.BRANCH_NAME}:${VERSION}"
                        bat "docker push gonzagomezp1/node${env.BRANCH_NAME}:${VERSION}"
                    }
                }
            }
        }
    }
    }
