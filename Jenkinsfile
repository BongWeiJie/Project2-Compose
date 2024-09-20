pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
        NPM_CONFIG_CACHE = '/tmp/.npm'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    def installOutput = sh(script: 'npm install --save', returnStdout: true)
                    echo installOutput
                    writeFile file: 'install-dependencies.log', text: installOutput
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    echo 'Running Snyk security scan...'
                    sh 'npm install snyk'
                    def snykOutput = sh(script: './node_modules/.bin/snyk test --all-projects || true', returnStdout: true)
                    echo snykOutput
                    writeFile file: 'snyk-report.log', text: snykOutput
                    
                    if (snykOutput.contains('Critical')) {
                        error("Critical vulnerabilities detected during the Snyk scan.")
                    }
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
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    def testOutput = sh(script: 'npm test', returnStdout: true)
                    echo testOutput
                    writeFile file: 'test-report.log', text: testOutput
                    
                    if (testOutput.contains('failed')) {
                        error("Tests failed during execution.")
                    }
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
    }
}
