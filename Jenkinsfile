pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    stages {
        stage('docker-build-test-base') {
            steps {
                echo 'Builing base image for api-tests'
                sh "docker build -t teodorajovcheska7/api-tests-base . -f Dockerfile.base"

            }
        }
        stage('docker-build-test-runner') {
            steps {
                echo 'Building runner image for api-tests'
                sh "docker build -t teodorajovcheska7/api-tests-runner . -f Dockerfile.runner"
            }
        }
    }
}
