pipeline {
    agent any

    environment {
        MONGO_URI = credentials('MONGO_URI') // Mongo URI from Jenkins credentials
        EC2_PUBLIC_IP = '18.212.231.42' // Replace with your deployed EC2 public IP
        APP_PORT = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/alishasaiyed7/Jenkins_project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test' // Make sure "test" is defined in package.json
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i ~/Jenkins_erver.pem ubuntu@18.212.231.42 << 'EOF'
                    if [ ! -d "Jenkins_project" ]; then
                        git clone https://github.com/alishasaiyed7/Jenkins_project.git
                    fi
                    cd Jenkins_project
                    git pull origin main
                    npm install
                    pm2 delete all || true
                    pm2 start app.js
EOF
                    '''
                }
            }
        }
        stage('Show App URL') {
            steps {
                echo "✅ Your app is running at: http://$EC2_PUBLIC_IP:$APP_PORT"
                //comment
            }
        }
    }
}
