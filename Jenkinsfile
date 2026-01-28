pipeline {
    agent any

    tools {
        nodejs 'Node18'   // Jenkins → Global Tool Configuration
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    environment {
        NODE_ENV = 'production'
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                  npm ci
                '''
            }
        }

        stage('Security Audit (Non-blocking)') {
            steps {
                sh '''
                  npm audit || true
                '''
            }
        }

        stage('Build Application') {
            steps {
                sh '''
                  npm run build
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build completed successfully"
        }

        failure {
            echo "❌ Build failed"
        }

        always {
            cleanWs()
        }
    }
}
