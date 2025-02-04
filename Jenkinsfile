pipeline {
    agent any
    environment {
        EC2_USER = "ec2-user"
        EC2_HOST = "<EC2-PUBLIC-IP>"
        EC2_DIR  = "/home/ec2-user/app"
        GIT_REPO = "git@github.com:your-repo.git"
        GIT_BRANCH = "main"
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    try {
                        git branch: "${GIT_BRANCH}", credentialsId: 'your-credential-id', url: "${GIT_REPO}"
                    } catch (Exception e) {
                        error "Git checkout failed! Check credentials or repository access."
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        sh 'npm install'  // Modify for different languages (e.g., Maven, Gradle, etc.)
                    } catch (Exception e) {
                        error "Build failed! Check dependencies or package issues."
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['your-credential-id']) {
                    script {
                        try {
                            sh """
                            ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} << 'EOF'
                            set -e
                            echo "Navigating to application directory..."
                            cd ${EC2_DIR}

                            echo "Fetching latest code..."
                            git pull origin ${GIT_BRANCH}

                            echo "Installing dependencies..."
                            npm install

                            echo "Restarting application..."
                            pm2 restart all || pm2 start app.js --name "my-app"

                            echo "Deployment complete!"
                            EOF
                            """
                        } catch (Exception e) {
                            error "Deployment failed! Check logs on the EC2 instance."
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed! Check the logs for errors."
        }
    }
}

