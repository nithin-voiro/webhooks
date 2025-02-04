pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'your-credential-id', url: 'git@github.com:your-repo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'  // Change for different applications
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['your-credential-id']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ec2-user@<EC2-PUBLIC-IP> '
                    cd /home/ec2-user/app &&
                    git pull origin main &&
                    npm install &&
                    pm2 restart all
                    '
                    """
                }
            }
        }
    }
}
