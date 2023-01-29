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
                        //def image = docker.build("erivaldolopes/kube-news:v1")
                        dockerapp.push("latest")
                    }
                }
            }
    }

//    stage ('Push Docker Image') {
//        steps {
//            script {
//                docker.withRegistry("https://registry.hub.docker.com", 'registry_id')
//                dockerapp.push('latest')
//                dockerapp.push("${env.BUILD_ID}")
//            }
//        }
//    }

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