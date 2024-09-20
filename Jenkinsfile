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
                    def installOutput = sh(script: 'npm install --save', returnStdout: true)
                    echo installOutput
                    writeFile file: 'install-dependencies.log', text: installOutput
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    sh 'npm install snyk' // Install Snyk locally
                    def snykOutput = sh(script: './node_modules/.bin/snyk test --all-projects || true', returnStdout: true)
                    echo snykOutput
                    writeFile file: 'snyk-report.log', text: snykOutput
                }
            }
            post {
                always {
                    echo 'Snyk scan completed.'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    // Add build commands here if applicable
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    def testOutput = sh(script: 'npm test', returnStdout: true)
                    echo testOutput
                    writeFile file: 'test-report.log', text: testOutput
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying the project...'
                    // Add deployment commands here if applicable
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
            archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
