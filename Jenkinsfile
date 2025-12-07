pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stages {

        stage('Test') {
            steps {
                sh "go test ./..."
            }
        }

        stage('Build') {
            steps {
                sh "go build -o main main.go"
                archiveArtifacts artifacts: 'main', fingerprint: true
            }
        }

        stage('Deploy Artifact') {
            steps {
                sshagent(['my-ssh-credentials']) {
                    sh """
                        scp -o StrictHostKeyChecking=no main user@target:/opt/myapp/main
                        ssh user@target '
                            sudo systemctl daemon-reload
                            sudo systemctl enable myapp.service
                            sudo systemctl restart myapp.service
                        '
                    """
                }
            }
        }
    }
}
