pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
        NPM_CONFIG_CACHE = '/tmp/.npm'
        AWS_DEFAULT_REGION = 'your-aws-region'
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        APPLICATION_NAME = 'aws-elastic-beanstalk-express-js-sample'
        ENVIRONMENT_NAME = 'your-environment-name'
        S3_BUCKET = 'your-s3-bucket-for-uploads'
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
        stage('Package Application') {
            steps {
                script {
                    echo 'Packaging the application...'
                    sh 'zip -r app.zip .'
                }
            }
        }
        stage('Upload to S3') {
            steps {
                script {
                    echo 'Uploading to S3...'
                    sh "aws s3 cp app.zip s3://$S3_BUCKET/app.zip"
                }
            }
        }
        stage('Deploy to Elastic Beanstalk') {
            steps {
                script {
                    echo 'Deploying to Elastic Beanstalk...'
                    def versionLabel = "${env.BUILD_ID}-${new Date().format('yyyyMMddHHmmss')}"
                    sh """
                        aws elasticbeanstalk create-application-version \
                            --application-name $APPLICATION_NAME \
                            --version-label $versionLabel \
                            --source-bundle S3Bucket=$S3_BUCKET,S3Key=app.zip

                        aws elasticbeanstalk update-environment \
                            --environment-name $ENVIRONMENT_NAME \
                            --version-label $versionLabel
                    """
                }
            }
        }
    }
    post {
        always {
            node { // Wrap this in a node block
                echo 'Cleaning up...'
                archiveArtifacts artifacts: '**/*.log', allowEmptyArchive: true
            }
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
