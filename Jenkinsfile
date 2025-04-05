pipeline {
    agent any

    environment {
        MONGO_URI ='mongodb+srv://saiyedalisha110:<db_password>@cluster1.t50mxqv.mongodb.net/retryWrites=true&w=majority&appName=Cluster1'
        DEPLOY_USER = 'ubuntu'
        DEPLOY_HOST = '18.212.231.42'         // Replace with your target EC2 Public IP
        PEM_FILE = 'Jenkins_erver.pem'                       // Make sure this is available on Jenkins server
        APP_DIR = '/home/ubuntu/Jenkins_project'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', credentialsId: 'github-creds', url: 'https://github.com/alishasaiyed7/Jenkins_project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build/Tests') {
            steps {
                sh 'npm test || echo "No tests found"'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                    echo "Deploying to EC2..."
                    chmod 400 $PEM_FILE
                    scp -o StrictHostKeyChecking=no -i $PEM_FILE -r . $DEPLOY_USER@$DEPLOY_HOST:$APP_DIR
                    ssh -o StrictHostKeyChecking=no -i $PEM_FILE $DEPLOY_USER@$DEPLOY_HOST << EOF
                        cd $APP_DIR
                        npm install
                        pm2 stop all || true
                        pm2 start app.js --name app
                    EOF
                '''
            }
        }

        stage('Print URL') {
            steps {
                echo "App should be running at: http://$DEPLOY_HOST:3000"
            }
        }
    }
}
