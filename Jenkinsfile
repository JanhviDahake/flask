pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Build') {
            steps {
                echo '📦 Creating virtual environment and installing dependencies...'
                sh '/usr/bin/python3 -m venv $VENV_DIR'
                sh '. $VENV_DIR/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo '✅ Running unit tests...'
                sh '. $VENV_DIR/bin/activate && pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying Flask app...'
                sh '. $VENV_DIR/bin/activate && nohup python app.py &'
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
            mail to: 'your-email@example.com',
                 subject: "✅ Build Successful - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Great news! The build and deployment succeeded.\n\nSee details: ${env.BUILD_URL}"
        }

        failure {
            echo '❌ Build or tests failed. Check logs.'
            mail to: 'your-email@example.com',
                 subject: "❌ Build Failed - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Unfortunately, the build failed. Check Jenkins logs:\n\n${env.BUILD_URL}"
        }
    }
}
