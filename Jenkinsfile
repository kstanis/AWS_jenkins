pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix://$run/docker.sock"
        STAGE_INSTANCE = "ubuntu@ip-10-0-0-144.eu-north-1.compute.internal"
    }
    stages {
        stage('Настройка SSH-туннеля') {
            steps {
                script {
                    // Используйте тройные кавычки для многострочных команд
                    sh """
                        ssh -nNT -L ${WORKSPACE}/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid
                        sleep 5
                    """
                }
            }
        }
        stage('Развертывание') {
            steps {
                script {
                    // Используйте DOCKER_HOST в качестве переменной окружения для команды 'docker'
                    sh docker ps -a"
                }
            }
        }
    }
    post {
        always {
            script {
                // Очистка: Завершение SSH-туннеля
                sh "pkill -F /tmp/tunnel.pid"
            }
        }
    }
}
