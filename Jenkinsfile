pipeline {
    agent any

    environment {
        DOCKER_USER = "gokumonkey"
        TOKEN = "dckr_pat_vHMjHr6LEPU-5_bZln8EdQf_HzY"
        IMAGE_NAME = "mywebsite"
        DOCKER_SERVER = "ubuntu@3.110.109.12"
    }

    stages {

        stage('Source') {
            steps {
                git branch: 'main',
                url: 'https://github.com/gokumonkey-36/jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                ssh ${DOCKER_SERVER} "
                    mkdir -p ~/website
                "

                scp -r ./* ${DOCKER_SERVER}:~/website/

                ssh ${DOCKER_SERVER} "
                    cd ~/website &&
                    docker build -t ${DOCKER_USER}/${IMAGE_NAME}:v2 . &&
                    docker login -u ${DOCKER_USER} -p ${TOKEN} &&
                    docker push ${DOCKER_USER}/${IMAGE_NAME}:v2
                "
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Skipping tests for now'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                ssh ${DOCKER_SERVER} "
                    cd ~/website &&

                    kubectl apply -f deployment.yaml
                "
                '''
            }
        }
    }
}
