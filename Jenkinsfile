pipeline {
    agent any

    stages ('Build Docker Image') {
        steps {
            script {
                dockerapp = docker.build("erivaldolopes/kubenews:${env.BUILD_ID}", '-f .src/Dockerfile .src')
            }
        }

    }
}