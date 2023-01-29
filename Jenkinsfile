pipeline {
    agent any

    stages {

    stage ('Build Docker Image') {
        steps {
            script {
                dockerapp = docker.build("erivaldolopes/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
            }
        }

    }

    stage('Build and push Docker image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'registry_id', url: 'https://index.docker.io/v1/']) {
                        dockerapp.push("${env.BUILD_ID}")
                        dockerapp.push("latest")
                    }
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