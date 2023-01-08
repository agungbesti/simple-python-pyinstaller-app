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
        
    withDockerContainer('cdrx/pyinstaller-linux:python2'){
        stage('Deliver') {
            echo '=========[stage Deployment]==========='
            checkout scm
            sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-agungbesti/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
            sh 'pyinstaller --onefile sources/add2vals.py'
            archiveArtifacts 'dist/add2vals'
            sh 'docker run --rm -v /var/jenkins_home/workspace/submission-cicd-pipeline-agungbesti/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
        }
    }
}
