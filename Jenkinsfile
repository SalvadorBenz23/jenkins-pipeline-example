pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add build steps here, such as compiling code or running scripts.
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add test steps here, such as running unit tests or integration tests.
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                // Add deployment steps here.
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
