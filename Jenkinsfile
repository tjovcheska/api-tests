pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    environment {
        DOCKER_USERNAME = credentials('docker-username')
        DOCKER_PASSWORD = credentials('docker-password')
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
                sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                sh "docker build --no-cache -t teodorajovcheska7/api-tests-base . -f Dockerfile.base"
                sh "docker push teodorajovcheska7/api-tests-base:latest"
            }
        }
        stage('docker-build-test-runner') {
            steps {
                echo 'Building runner image for api-tests'
                sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                sh "docker build --no-cache -t teodorajovcheska7/api-tests-runner . -f Dockerfile.runner"
                sh "docker push teodorajovcheska7/api-tests-runner:latest"
            }
        }
    }
}
