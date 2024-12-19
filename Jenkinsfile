pipeline {
    agent any

    environment {
        PROJECT_NAME = "Pipeline Project"
        PYTHON_IMAGE = "python:3.8-alpine3.16"
        TEST_IMAGE = "grihabor/pytest"
        DEPLOY_IMAGE = "cdrx/pyinstaller-linux"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the source code..."
                checkout scm
            }
        }

        stage('Build') {
            agent {
                docker {
                    image "${PYTHON_IMAGE}"
                }
            }
            steps {
                echo 'Building the project...'
                sh '''
                    echo "Simulating source creation..."
                    mkdir -p sources
                    echo "def add(a, b): return a + b" > sources/calc.py
                    echo "if __name__ == '__main__': print('Hello World')" > sources/prog.py

                    echo "Compiling Python source files..."
                    python3 -m py_compile sources/prog.py sources/calc.py
                '''
                stash name: 'build-output', includes: 'sources/**/*.pyc'
            }
        }

        stage('Test') {
            agent {
                docker {
                    image "${TEST_IMAGE}"
                }
            }
            steps {
                echo 'Running tests...'
                unstash 'build-output'
                sh '''
                    echo "Creating test file..."
                    echo "from calc import add; def test_add(): assert add(1, 2) == 3" > sources/test_calc.py
                    
                    echo "Running pytest..."
                    pytest -v --junit-xml test-reports/results.xml sources/test_calc.py
                '''
                junit 'test-reports/results.xml'
                stash name: 'test-results', includes: 'test-reports/*.xml'
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image "${DEPLOY_IMAGE}"
                }
            }
            steps {
                echo 'Deploying the project...'
                unstash 'test-results'
                sh '''
                    echo "Packaging application..."
                    pyinstaller --onefile sources/prog.py

                    echo "Deployment artifact:"
                    ls -lh dist/
                '''
                archiveArtifacts artifacts: 'dist/prog', fingerprint: true
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
