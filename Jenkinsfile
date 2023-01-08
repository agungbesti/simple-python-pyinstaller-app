node {
    withDockerContainer('python:2-alpine'){
        stage('Build') {
            echo '=========[stage build]==========='
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    withDockerContainer('qnib/pytest'){
        stage('Test') {
            echo '=========[stage Testing]==========='
            checkout scm
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
            }
        }
}
