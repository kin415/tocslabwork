pipeline {
    agent any

    stages {
        stage('Deploy HTML') {
            steps {
                sshagent(['apache']) {
                    script {
                        echo 'Deploying index.html to remote Apache server...'

                        // Automatically add the server's SSH key to known_hosts
                        sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H ip-172-31-24-16 >> ~/.ssh/known_hosts
                        chmod 644 ~/.ssh/known_hosts
                        '''

                        // Copy the file to the remote server
                        sh 'scp index.html ubuntu@ip-172-31-24-16:/var/www/html/'
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying index.html deployment on remote server...'
                    sh 'curl http://ip-172-31-24-16/index.html'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs for more details.'
        }
    }
}
