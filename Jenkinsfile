pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    docker.image('python:2-alpine').inside {
                        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('qnib/pytest').inside {
                        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
                    }
                }
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                script {
                    docker.image('cdrx/pyinstaller-linux:python2').inside {
                        sh 'pyinstaller --onefile sources/add2vals.py'
                    }
                }
            }
        }
    }
}
