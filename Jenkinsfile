pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile add2vals.py calc.py'
                stash(name: 'compiled-results', includes: '*.py*')
                sh 'ls -l'
            }
        }
        stage('Test') { 
            agent {
                docker {
                    image 'qnib/pytest' 
                }
            }
            steps {
                sh 'py.test --junit-xml test-reports/results.xml test_calc.py' 
                sh 'cat test-reports/results.xml'
            }
            post {
                always {
                    junit 'test-reports/results.xml' 
                }
            }
        }
    }
}
