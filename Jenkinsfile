pipeline {
    agent any

    environment {
        MONGO_URI = credentials('MONGO_URI') // Add this in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/alishasaiyed7/Jenkins_project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test' // Ensure your project has test scripts
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@18.212.231.42 << EOF
                    cd Jenkins_project
                    git pull origin main
                    npm install
                    pm2 restart all
                    EOF
                    '''
                }
            }
        }
    }
}
