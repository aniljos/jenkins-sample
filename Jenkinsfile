pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'v20.11.1' // Name must match the NodeJS configuration in Jenkins
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git branch: 'main', url: 'https://github.com/aniljos/jenkins-sample'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies
                //sh 'npm install'
                bat 'npm install'
            }
        }
        stage('Build') {
            steps {
                // Build the React project
                //sh 'npm run build'
                bat 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                //sh 'npm test'
                bat 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def sourcePath = "${env.WORKSPACE}\\build" // Path to the build output directory
                    def destinationPath = "D:\\Jenkins\\React\\builds" // Target directory for deployment

                    echo "Copying files from ${sourcePath} to ${destinationPath}"
                    bat "xcopy /s /e /y ${sourcePath}\\* ${destinationPath}\\"
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            // Notify success
            echo 'Build and deployment successful!'
        }
        failure {
            // Notify failure
            echo 'Build or deployment failed.'
        }
    }
}
