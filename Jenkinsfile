pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Your build commands here
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Your test commands here
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
            mail to: 'noreply@yourproject.test',
                 subject: "✅ Build Successful - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Great news! The build and deployment succeeded.\n\nSee details: ${env.BUILD_URL}",
                 from: 'noreply@yourproject.test'
        }
        failure {
            echo '❌ Build failed.'
            mail to: 'noreply@yourproject.test',
                 subject: "❌ Build Failed - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Unfortunately, the build failed.\n\nCheck details: ${env.BUILD_URL}",
                 from: 'noreply@yourproject.test'
        }
    }
}
