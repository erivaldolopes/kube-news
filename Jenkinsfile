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
        environment {
                tag_version = "${env.BUILD_ID}"
        }
        steps {
            script {
                withKubeConfig([credentialsId: 'do_kubeconfig']) {
                    sh 'sed -i "s/{{TAG}}/$tag_version/g" .kubernetes/*'
                    sh 'kubectl apply -f ./kubernetes/'
                }                
            }
        }
    }

    }
}