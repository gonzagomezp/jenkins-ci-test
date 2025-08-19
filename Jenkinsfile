pipeline {
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
                    echo "${env.BRANCH_NAME}"
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
                bat 'docker build -t image1 .'
            }
        }

        /* stage('deploy') {
            steps {
                echo 'Deploying...'
                // commands to deploy your app
            }
        } */
    }
}
