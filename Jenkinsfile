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
                sh 'cp index.html build/'
                echo "Build completed"
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
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
