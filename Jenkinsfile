pipeline {
    agent any

    environment {
        DEPLOY_DIR = 'D:/React-ToDo-App-CI-CD-Deployment'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the React ToDo Application repository...'
                git 'https://github.com/feelinganubhav/React-ToDo-App.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Lint Code') {
            steps {
                echo 'Linting the code using ESLint...'
                sh 'npx eslint . || true'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests On All React ToDo App Fuctionalities using Jest...'
                sh 'npm test || true'
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building the React ToDo Application...'
                sh 'npm run build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the application...'
                sh "rm -rf ${DEPLOY_DIR}/*"
                sh "cp -r dist/* ${DEPLOY_DIR}/"
            }
        }

        stage('Start Server for Testing') {
            steps {
                echo 'Starting the Server...'
                sh 'npx serve -s build -l 3000 &'
            }
        }

        stage('Post-deployment Testing') {
            steps {
                script {
                    def responseCode = sh(
                        script: "curl -o /dev/null -s -w '%{http_code}' http://localhost:3000",
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
            sh "pkill -f 'serve -s build -l 3000' || true"
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
