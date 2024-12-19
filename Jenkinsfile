pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Cloning the repository and building the project...'
                sh '''
                    echo "Cloning the repository..."
                    git clone --depth 1 https://github.com/SalvadorBenz23/jenkins-pipeline-example.git repo
                    cd repo

                    echo "Simulating a build step..."
                    mkdir -p build
                    echo "Build output" > build/output.txt
                '''
                stash includes: 'repo/build/**', name: 'build-files'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                unstash 'build-files'
                sh '''
                    cd repo
                    echo "Simulating a test step..."
                    echo "Tests passed!" > build/test-results.txt
                '''
                stash includes: 'repo/build/test-results.txt', name: 'test-results'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
                unstash 'test-results'
                sh '''
                    cd repo
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
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
