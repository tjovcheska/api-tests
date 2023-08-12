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
                build-docker-image("teodorajovcheska7/api-tests-base:latest", "Dockerfile.base")
                // echo 'Builing base image for api-tests'
                // sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                // sh "docker build --no-cache -t teodorajovcheska7/api-tests-base . -f Dockerfile.base"
                // sh "docker push teodorajovcheska7/api-tests-base:latest"
            }
        }
        stage('docker-build-test-runner') {
            steps {
                build-docker-image("teodorajovcheska7/api-tests-runner:latest", "Dockerfile.runner")
                // echo 'Building runner image for api-tests'
                // sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                // sh "docker build --no-cache -t teodorajovcheska7/api-tests-runner . -f Dockerfile.runner"
                // sh "docker push teodorajovcheska7/api-tests-runner:latest"
            }
        }
    }
}

def build-docker-image(String tag, String dockerfile) {
    echo "Building ${tag} image for api-tests based on ${dockerfile}"
    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
    sh "docker build --no-cache -t ${tag} . -f ${dockerfile}"
    sh "docker push ${tag}"
}