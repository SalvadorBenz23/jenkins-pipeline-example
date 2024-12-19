pipeline {
    agent any

    stages {
        // Checkout stage
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/SalvadorBenz23/jenkins-pipeline-example.git']]
                ])
            }
        }

        // Build stage
        stage('Build') {
            agent {
                docker {
                    image 'python:3.8-alpine3.16'
                    args '--user root' // Run as root to allow installations
                }
            }
            steps {
                echo 'Building the application...'
                sh '''
                    python3 --version
                    echo "Simulating build step"
                '''
            }
        }

        // Test stage
        stage('Test') {
            agent {
                docker {
                    image 'grihabor/pytest'
                }
            }
            steps {
                echo 'Running tests...'
                sh '''
                    echo "Simulating test step"
                '''
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit 'test-reports/results.xml'
                }
            }
        }

        // Deploy stage
        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                sh '''
                    echo "Simulating deploy step"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
