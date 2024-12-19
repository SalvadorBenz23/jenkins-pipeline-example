pipeline {
    agent none

    environment {
        PYTHON_IMAGE = 'python:3.8-alpine3.16'
        PYTEST_IMAGE = 'grihabor/pytest'
        PYINSTALLER_IMAGE = 'cdrx/pyinstaller-linux'
    }

    stages {
        stage('Build') {
            agent {
                docker { image PYTHON_IMAGE }
            }
            steps {
                echo 'Starting Build Stage...'
                sh '''
                    mkdir -p sources
                    echo 'def add(a, b): return a + b' > sources/calc.py
                    echo 'if __name__ == "__main__": print("Hello World")' > sources/prog.py
                    python3.8 -m py_compile sources/prog.py sources/calc.py
                    ls -R sources/__pycache__
                '''
                stash name: 'compiled-results', includes: 'sources/*.py*'
            }
        }

        stage('Test') {
            agent {
                docker { image PYTEST_IMAGE }
            }
            steps {
                echo 'Starting Test Stage...'
                unstash 'compiled-results'
                sh '''
                    mkdir -p test-reports
                    echo 'from calc import add; def test_add(): assert add(1, 2) == 3' > sources/test_calc.py
                    pytest -v --junit-xml test-reports/results.xml sources/test_calc.py || true
                    ls -l test-reports
                '''
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit 'test-reports/results.xml'
                }
            }
        }

        stage('Deliver') {
            agent any
            steps {
                echo 'Starting Deliver Stage...'
                dir("${env.WORKSPACE}/build") {
                    unstash 'compiled-results'
                    sh '''
                        docker run --rm \
                        -v ${WORKSPACE}/sources:/src \
                        ${PYINSTALLER_IMAGE} \
                        "pyinstaller -F prog.py"
                    '''
                }
            }
            post {
                success {
                    echo 'Archiving deliverable...'
                    archiveArtifacts artifacts: 'build/dist/prog'
                }
                cleanup {
                    cleanWs()
                }
            }
        }
    }
}
