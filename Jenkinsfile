pipeline {
    agent {
        docker {
            image 'node:16'
            label 'docker'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  // Use Jenkins credentials plugin for security
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies using npm
                    sh 'npm install'
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Install Snyk CLI
                    sh 'npm install -g snyk'
                    
                    // Run Snyk test to scan for vulnerabilities
                    sh 'snyk test --all-projects'
                }
            }
            post {
                always {
                    sh 'snyk monitor'
                }
                failure {
                    echo 'Snyk scan found vulnerabilities. Failing the build.'
                    currentBuild.result = 'FAILURE'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying the project...'
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
