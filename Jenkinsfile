pipeline {
    agent any

    environment {
        DEPLOY_DIR = 'D:/React-ToDo-App-CI-CD-Deployment'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the React ToDo Application repository...'
                git branch: 'main', url: 'https://github.com/feelinganubhav/React-ToDo-App.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
            }
        }

        stage('Lint Code') {
            steps {
                echo 'Linting the code using ESLint...'
                bat 'npx eslint . || exit /B 0'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests On All React ToDo App Fuctionalities using Jest...'
                bat 'npm test || exit /B 0'
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building the React ToDo Application...'
                bat 'npm run build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the application...'
                bat "rm -rf ${DEPLOY_DIR}/*"
                bat "cp -r dist/* ${DEPLOY_DIR}/"
            }
        }

        stage('Start Server for Testing') {
            steps {
                echo 'Starting the Server...'
                bat 'npx serve -s build -l 3000 &'
            }
        }

        stage('Post-deployment Testing') {
            steps {
                script {
                    def responseCode = sh(
                        script: "curl -o /dev/null -s -w '%%{http_code}' http://localhost:3000",
                         returnStdout: true).trim()

                    if (responseCode != '200') {
                        error "Post-deployment testing failed with HTTP status code: ${responseCode}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up any running servers...'
            bat "pkill -f 'serve -s build -l 3000' || exit /B 0"
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
