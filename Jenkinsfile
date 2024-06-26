pipeline {
    agent any

    environment {
        NODEJS_HOME = tool name: 'v20.11.1' // Name must match the NodeJS configuration in Jenkins
        PATH = "${NODEJS_HOME}/bin:${env.PATH}"
        IMAGE_NAME = 'sample-react-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        SAVE_PATH = 'D:\\Jenkins\\React\\docker-images' // Update this to your desired save path
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
         stage('Package') {
            steps {
                script {
                    docker.build("${env.IMAGE_NAME}:${env.IMAGE_TAG}")
                }
            }
        }
        stage('Save Image') {
            steps {
                script {
                    def imageTar = "${env.WORKSPACE}\\${env.IMAGE_NAME}_${env.IMAGE_TAG}.tar"
                    bat "docker save -o ${imageTar} ${env.IMAGE_NAME}:${env.IMAGE_TAG}"
                    bat "move ${imageTar} ${env.SAVE_PATH}"
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
