pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix://\$(pwd)/docker.sock"
        STAGE_INSTANCE = "ubuntu@16.171.144.230"
    }
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {

                    sh "whoami"
                    sh "ssh -i /var/lib/jenkins/id_rsa -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
                    // Иногда не достаточно времени для создания туннеля, добавим паузу
                    sleep 5
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo ${DOCKER_HOST}
                    sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
                }
            }
        }
    }
    post {
        always {
            script {
                sh "pkill -F /tmp/tunnel.pid" & "rm /tmp/tunnel.pid"
            }
        }
    }
}
