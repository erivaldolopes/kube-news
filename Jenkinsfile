pipeline {
    agent any

    stages {

    stage ('Build Docker Image') {
        steps {
            script {
                dockerapp = docker.build("erivaldolopes/kubenews:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
            }
        }

    }

    stage ('Deploy Kubernetes') {
        steps {
            script {
                withKubeConfig([credentialsId: 'do_kubeconfig']) {
                    sh 'kubectl apply -f ./kubernetes/'
                }                
            }
        }
    }

    }
}