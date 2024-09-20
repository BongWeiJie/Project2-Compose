pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
        NPM_CONFIG_CACHE = '/tmp/.npm' // Set custom npm cache directory
        AWS_DEFAULT_REGION = 'your-aws-region' // e.g., 'us-west-2'
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id') // Add your AWS access key in Jenkins credentials
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key') // Add your AWS secret key in Jenkins credentials
        APPLICATION_NAME = 'aws-elastic-beanstalk-express-js-sample'
        ENVIRONMENT_NAME = 'your-environment-name'
        S3_BUCKET = 'your-s3-bucket-for-uploads'
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
                    // Assuming there's a build step defined in package.json, e.g., "build": "webpack"
                    // Uncomment the line below if you have a build command
                    // sh 'npm run build'
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
        stage('Package Application') {
            steps {
                script {
                    sh 'zip -r app.zip .'
                }
            }
        }
        stage('Upload to S3') {
            steps {
                script {
                    sh "aws s3 cp app.zip s3://$S3_BUCKET/app.zip"
                }
            }
        }
        stage('Deploy to Elastic Beanstalk') {
            steps {
                script {
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
            node {
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
