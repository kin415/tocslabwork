pipeline {
    agent any

    stages {
        stage('Deploy Files') {
            steps {
                sshagent(['apache']) {
                    script {
                        echo 'Deploying files to remote Apache server...'

                        // Automatically add the server's SSH key to known_hosts
                        sh '''
                        mkdir -p ~/.ssh
                        ssh-keyscan -H ip-172-31-23-190 >> ~/.ssh/known_hosts
                        chmod 644 ~/.ssh/known_hosts
                        '''

                        // Copy the files to the remote server
                        sh '''
                        scp index.html ubuntu@ip-172-31-23-190:/var/www/html/
                        scp script.js ubuntu@ip-172-31-23-190:/var/www/html/
                        scp style.css ubuntu@ip-172-31-23-190:/var/www/html/
                        '''
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying file deployment on remote server...'
                    
                    // Verify index.html
                    echo 'Verifying index.html...'
                    sh 'curl http://ip-172-31-23-190/index.html'
                    
                    // Verify script.js
                    echo 'Verifying script.js...'
                    sh 'curl http://ip-172-31-23-190/script.js'
                    
                    // Verify style.css
                    echo 'Verifying style.css...'
                    sh 'curl http://ip-172-31-23-190/style.css'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment of all files completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs for more details.'
        }
    }
}
