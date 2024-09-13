pipeline {
    agent {
        docker {
            image 'node:16'  // Use Node.js 16 Docker image as the build agent
            args '-v $HOME/.npm:/root/.npm'  // Cache npm modules
        }
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies using npm
                    sh 'npm install --save'
                }
            }
        }

    }

    post {
        always {
            // Clean up any resources, notify build status, etc.
            echo 'Pipeline finished.'
        }
    }
}
