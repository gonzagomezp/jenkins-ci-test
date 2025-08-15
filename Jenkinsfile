pipeline {
    agent any              // where to run the pipeline (any available node)

    environment {
        // optional environment variables
        MY_VAR = "hello"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building app..."
                mkdir build && echo "build artifact" > build/app.txt
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                cat build/app.txt
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // commands to deploy your app
            }
        }
    }

    post {
        always {
            echo 'This always runs, e.g., cleanup'
        }
        success {
            echo 'This runs if the pipeline succeeded'
        }
        failure {
            echo 'This runs if the pipeline failed'
        }
    }
}
