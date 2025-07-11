pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        SLACK_CHANNEL = '#all-herowired'
        SLACK_TOKEN = credentials('slack-token') // Add this secret in Jenkins > Credentials
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
                sh 'ls -l' // Debug: See what files exist
                // Update "app.py" if your entrypoint has a different name
                sh '. $VENV_DIR/bin/activate && nohup python app.py &'
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
            sh """
                curl -X POST -H "Authorization: Bearer $SLACK_TOKEN" -H "Content-type: application/json" \
                --data '{"channel":"$SLACK_CHANNEL","text":"✅ Jenkins Pipeline SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}"}' \
                https://slack.com/api/chat.postMessage
            """
        }

        failure {
            echo '❌ Build or tests failed. Check logs.'
            sh """
                curl -X POST -H "Authorization: Bearer $SLACK_TOKEN" -H "Content-type: application/json" \
                --data '{"channel":"$SLACK_CHANNEL","text":"❌ Jenkins Pipeline FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}"}' \
                https://slack.com/api/chat.postMessage
            """
        }
    }
}
