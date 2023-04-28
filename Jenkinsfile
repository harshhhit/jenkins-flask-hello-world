pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'python--version'
                sh 'install Python 3.10.5'
                sh 'pip install -r requirements.txt'
            }
        }
        
        stage('Test') {
            steps {
                sh 'python -m pytest tests/'
            }
        }
        
        stage('Deploy') {
            environment {
                SERVER = 'example.com'
                USERNAME = credentials('ssh-username')
                PASSWORD = credentials('ssh-password')
            }
            steps {
                sshagent(['ssh-username']) {
                    sh "sshpass -p ${PASSWORD} scp -r * ${USERNAME}@${SERVER}:/var/www/app"
                    sh "sshpass -p ${PASSWORD} ssh ${USERNAME}@${SERVER} 'cd /var/www/app && sudo systemctl restart app'"
                }
            }
        }
    }
}
