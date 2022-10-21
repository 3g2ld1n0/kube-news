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

    }
}