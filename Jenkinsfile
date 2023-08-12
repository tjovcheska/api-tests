pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    stages {
        stage('docker-build-test-base') {
            when {
                anyOf {
                    changeset 'Gemfile'
                    changeset 'Dockerfile.base'
                    changeset 'Jenkinsfile'
                }
            }
            steps {
                echo 'Builing base image for api-tests'
                sh "docker build --no-cache -t teodorajovcheska7/api-tests-base . -f Dockerfile.base"
                sh "docker login -u teodorajovcheska -p randompass"
                sh "docker push teodorajovcheska7/api-tests-base:latest"
            }
        }
        stage('docker-build-test-runner') {
            steps {
                echo 'Building runner image for api-tests'
                sh "docker build --no-cache -t teodorajovcheska7/api-tests-runner . -f Dockerfile.runner"
                sh "docker login -u teodorajovcheska -p randompass"
                sh "docker push teodorajovcheska7/api-tests-runner:latest"
            }
        }
    }
}
