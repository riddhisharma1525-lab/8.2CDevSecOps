pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/riddhisharma1525-lab/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
            post {
                always {
                    emailext(
                        to: 'riddhi.sharma1525@gmail.com',
                        subject: "Jenkins - Run Tests - ${currentBuild.currentResult}",
                        body: "The Run Tests stage has completed with status: ${currentBuild.currentResult}.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
            post {
                always {
                    emailext(
                        to: 'riddhi.sharma1525@gmail.com',
                        subject: "Jenkins - NPM Audit Security Scan - ${currentBuild.currentResult}",
                        body: "The NPM Audit security scan has completed with status: ${currentBuild.currentResult}.",
                        attachLog: true
                    )
                }
            }
        }
    }
}
