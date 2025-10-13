pipeline {
    agent any

    environment {
        NODEJS_HOME = '/usr/bin'
        PATH = "$NODEJS_HOME:$PATH"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Rohan200428/Expense-Tracker.git'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend/expense-tracker') {
                    sh "rm -rf node_modules"
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy Backend') {
            steps {
                dir('backend') {
                    // Ensure PM2 starts from the correct directory
                    sh '''
                    pm2 delete backend || true
                    pm2 start server.js --name backend --cwd $(pwd)
                    pm2 save
                    '''
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                sudo rm -rf /usr/share/nginx/html/*
                sudo cp -r frontend/expense-tracker/dist/* /usr/share/nginx/html/
                ls -l /usr/share/nginx/html/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
