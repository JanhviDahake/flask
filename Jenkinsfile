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
            mail to: 'janhvidahake2001@gmail.com',
                subject: "✅ SUCCESS: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: "The pipeline completed successfully!\n\nCheck details at: ${env.BUILD_URL}"
        }

        failure {
            echo '❌ Build or tests failed. Check logs.'
            mail to: 'janhvidahake2001@gmail.com',
                subject: "❌ FAILURE: ${env.JOB_NAME} Build #${env.BUILD_NUMBER}",
                body: "The pipeline failed.\n\nCheck logs at: ${env.BUILD_URL}"
        }
    }
}
