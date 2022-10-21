pipeline {
    agent any

    stages {

        stage('Build Doker Imagem') {
            steps {
                script {
                    dockerapp = docker.build("egaldino/kube-news:$(ven.BUILD_ID)", '-f ./src/Dockerfile ./scr')
                }
            }            
        }

        stage('Push Docker Image') {
            steps {
                script {
                    dockerapp = docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
                        dockerapp.push('latest')
                        dockerapp.push("$(ven.BUILD_ID)")
                }
            }
        }

        state('Deploy Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh ' kubectrl apply -s ./k8s/deployment.yaml'
                }
            }
        }


    }
}