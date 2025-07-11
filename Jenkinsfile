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
            mail(
                to: 'janhvidahake2001@gmail.com',
                subject: "✅ Jenkins Build #${env.BUILD_NUMBER} SUCCESS - ${env.JOB_NAME}",
                body: """Good news! 🎉
Job: ${env.JOB_NAME}
Build #: ${env.BUILD_NUMBER}
Status: SUCCESS

Details: ${env.BUILD_URL}"""
            )
        }

        failure {
            echo '❌ Build or tests failed. Check logs.'
            mail(
                to: 'janhvidahake2001@gmail.com',
                subject: "❌ Jenkins Build #${env.BUILD_NUMBER} FAILED - ${env.JOB_NAME}",
                body: """Uh-oh! 🚨
Job: ${env.JOB_NAME}
Build #: ${env.BUILD_NUMBER}
Status: FAILED

Details: ${env.BUILD_URL}"""
            )
        }
    }
}
