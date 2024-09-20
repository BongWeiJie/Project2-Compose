pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
        NPM_CONFIG_CACHE = '/tmp/.npm' // Set custom npm cache directory
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install --save'
                }
            }
        }
        #stage('Snyk Security Scan') {
        #   steps {
        #        script {
        #            sh 'npm install snyk' // Install Snyk locally
        #            sh './node_modules/.bin/snyk test --all-projects' // Use local installation
        #        }
        #    }
        #    post {
        #        failure {
        #            error 'Snyk scan found vulnerabilities. Failing the build.'
        #        }
        #    }
        #}
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
