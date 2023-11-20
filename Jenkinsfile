pipeline {
    agent any
    environment {
        DOCKER_HOST = "unix://$run/docker.sock"
        STAGE_INSTANCE = "ubuntu@16.171.144.230"
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
                    sh "docker ps -a"
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
