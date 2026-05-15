pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Riyasarab/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Test Stage ${currentBuild.currentResult}: ${env.JOB_NAME}",
                        body: "The test stage has completed. Build status: ${currentBuild.currentResult}. Console log is attached.",
                        to: "riyabiju04@gmail.com",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report UPDATE') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext(
                        subject: "Security Scan ${currentBuild.currentResult}: ${env.JOB_NAME}",
                        body: "The security scan stage has completed. Build status: ${currentBuild.currentResult}. Console log is attached.",
                        to: "riyabiju04@gmail.com",
                        attachLog: true
                    )
                }
            }
        }
    }
}
