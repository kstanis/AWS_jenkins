pipeline {
    agent any

    stages {
        stage('Connect') {
            steps {
                script {
                    sh "ssh -i /var/lib/jenkins/jenkins_key -o StrictHostKeyChecking=no -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ubuntu@13.51.169.103 & echo \$! > /tmp/tunnel.pid"
                    
                    sleep 5
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'DOCKER_HOST=unix://\$(pwd)/docker.sock docker ps -a'
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'pkill -F /tmp/tunnel.pid & rm /var/lib/jenkins/workspace/AWS/docker.sock'
            }
        }
    }
}
