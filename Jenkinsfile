pipeline {
    agent any
    tools {
        nodejs 'nodejs-v22.12.0' // Use the configured Node.js tool in Jenkins
    }
    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs/' // Node.js path
        SONAR_SCANNER_PATH = 'C:/Users/srini/Downloads/sonar-scanner-cli-6.2.1.4610-windows-x64/sonar-scanner-6.2.1.4610-windows-x64/bin' // SonarQube scanner path
    }
    stages {
        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }
        stage('Lint') {
            steps {
                // Run linting to ensure code quality
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint
                '''
            }
        }
        stage('Build') {
            steps {
                // Build the React app
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Access the SonarQube token from Jenkins credentials
            }
            steps {
                // Run SonarQube analysis
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                sonar-scanner -Dsonar.projectKey=asg2 ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}


