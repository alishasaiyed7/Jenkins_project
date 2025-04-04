pipeline {
    agent any
    
    environment {
        MONGO_URI ='mongodb+srv://saiyedalisha110:<db_password>@cluster1.t50mxqv.mongodb.net/retryWrites=true&w=majority&appName=Cluster1'
        GOOGLE_CLIENT_ID = '362789764152-v7tices4ehmppfs117197g1853862vsv.apps.googleusercontent.com'
        GOOGLE_CLIENT_SECRET = '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'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/alishasaiyed7/Jenkins_project.git'
            }
        }

        stage('Setup Environment') {
            steps {
                script {
                    echo 'Setting up MongoDB...'
                    sh 'docker run --name mongodb -d -p 27017:27017 mongo'
                    echo 'Setting up Google Authentication...'
                    sh 'npm install google-auth-library'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building project...'
                    sh 'npm run build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm test'
                }
            }
        }

        // Remove or comment out the Deploy stage
        /*
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    sh 'ssh user@server "docker-compose up -d"'
                }
            }
        }
        */
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker stop mongodb && docker rm mongodb'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
