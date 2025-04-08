pipeline {
    agent any

    stages {
        stage('Pull Latest Code') {
            steps {
                git 'https://github.com/your-username/Jenkins_project.git'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['your-ec2-ssh-key']) {
                    sh '''
                    ssh ec2-user@54.163.202.164 'cd /home/ec2-user/app && git pull origin main && pm2 restart app'
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Code updated and app restarted!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
