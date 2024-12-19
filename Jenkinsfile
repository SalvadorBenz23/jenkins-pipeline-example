pipeline {
    agent any

    environment {
        PROJECT_NAME = "Pipeline Project"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the source code..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh '''
                echo "Simulating a build step..."
                mkdir -p build
                echo "Build output" > build/output.txt
                '''
                stash name: 'build-output', includes: 'build/**'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                unstash 'build-output'
                sh '''
                echo "Simulating a test step..."
                echo "Tests passed!" > build/test-results.txt
                '''
                stash name: 'test-results', includes: 'build/test-results.txt'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                unstash 'test-results'
                sh '''
                echo "Simulating deployment..."
                cat build/test-results.txt
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
