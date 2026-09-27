pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Checked out branch: ${env.BRANCH_NAME ?: 'main'}"
            }
        }

        stage('Build') {
            steps {
                echo "Starting build"
                sh 'mkdir -p build'
                script {
                    if (fileExists('index.html')) {
                        sh 'cp index.html build/'
                        env.BUILD_HAD_OUTPUT = 'true'
                    } else {
                        echo "index.html not found — skipping copy"
                        env.BUILD_HAD_OUTPUT = 'false'
                    }
                }
                echo "Build completed"
            }
        }

        stage('Archive') {
            when {
                environment name: 'BUILD_HAD_OUTPUT', value: 'true'
            }
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }

        stage('Archive skipped notice') {
            when {
                environment name: 'BUILD_HAD_OUTPUT', value: 'false'
            }
            steps {
                echo "Skipping archive — nothing was built"
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
        failure {
            echo "Pipeline failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}
