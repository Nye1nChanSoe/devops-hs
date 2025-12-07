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
                sh "go build main.go"
            }
        }
        stage('Deploy') {
            steps {
                sh """
                scp -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null \
                    main laborant@target:/opt/app/app
        
                # restart systemd service
                ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null laborant@target "
                    sudo systemctl daemon-reload
                    sudo systemctl restart app.service
                    sudo systemctl status app.service --no-pager
                "
                """
            }
        }
    }
}
