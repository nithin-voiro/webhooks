pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'your-credentials-id', url: 'git@github.com:your-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // Change as per your build tool (Maven, Gradle, npm, etc.)
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test' // Run unit tests
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    def ec2Ip = "<EC2-PUBLIC-IP>"
                    sh "scp -i ~/.ssh/id_rsa target/app.jar deploy@${ec2Ip}:/home/deploy/"
                    sh "ssh -i ~/.ssh/id_rsa deploy@${ec2Ip} 'cd /home/deploy && ./deploy.sh'"
                }
            }
        }
    }
}
