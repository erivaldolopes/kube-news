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

    stage ('Push Docker Image') {
        steps {
            script {
                docker.withRegistry('https://registry.hub.docker.com', [credentialsId: 'dockerhub'])
                dockerapp.push('latest')
                dockerapp.push("${env.BUILD_ID}")
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