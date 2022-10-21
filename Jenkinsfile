pipeline {
    agent any

    stages {

        stage('Build Doker Imagem') {
            steps {
                script {
                    dockerapp = docker.build("egaldino/kube-news:$(env.BUILD_ID)", '-f ./src/Dockerfile ./scr')
                }
            }            
        }

        stage('Push Docker Image') {
            steps {
                script {
                    dockerapp = docker.withRegistry('https://registry.hub.docker.com', 'dockerhub')
                        dockerapp.push('latest')
                        dockerapp.push("$(env.BUILD_ID)")
                }
            }
        }

        state('Deploy Kubernetes') {
            enviroment {
                tag_version = "$(env.BUILD_ID)"
            }
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh ' sed -i "{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh ' kubectrl apply -s ./k8s/deployment.yaml'
                }
            }
        }


    }
}