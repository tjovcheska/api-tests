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
                buildDockerImage("teodorajovcheska7/api-tests-base:latest", "Dockerfile.base")
            }
        }
        stage('docker-build-test-runner') {
            steps {
                buildDockerImage("teodorajovcheska7/api-tests-runner:latest", "Dockerfile.runner")
            }
        }
    }
}

def buildDockerImage(String tag, String dockerfile) {
    echo "Building ${tag} image for api-tests based on ${dockerfile}"
    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
    sh "docker build --no-cache -t ${tag} . -f ${dockerfile}"
    sh "docker push ${tag}"
}