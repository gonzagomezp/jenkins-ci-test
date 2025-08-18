pipeline {
    agent any              // where to run the pipeline (any available node)

    environment {
        // optional environment variables
        MY_VAR = "hello"
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
                npm install
                npm test
            }
        }

        stage('build') {
            steps {
                script {
                    npm install
                    npm build
                }
            }
        }
        
       /*  stage('build docker image') {
            steps {
                echo 'Deploying...'
                // commands to deploy your app
            }
        }

        stage('deploy') {
            steps {
                echo 'Deploying...'
                // commands to deploy your app
            }
        } */
    }
}
