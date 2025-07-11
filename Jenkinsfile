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
            emailext(
                subject: "✅ Jenkins Build #${env.BUILD_NUMBER} SUCCESS - ${env.JOB_NAME}",
                body: """<p>Good news! 🎉</p>
                         <p>Job: <b>${env.JOB_NAME}</b><br/>
                         Build #: <b>${env.BUILD_NUMBER}</b><br/>
                         Status: <b>SUCCESS</b></p>
                         <p>Details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'janhvidahake2001@gmail.com'
            )
        }

        failure {
            echo '❌ Build or tests failed. Check logs.'
            emailext(
                subject: "❌ Jenkins Build #${env.BUILD_NUMBER} FAILED - ${env.JOB_NAME}",
                body: """<p>Uh-oh! 🚨</p>
                         <p>Job: <b>${env.JOB_NAME}</b><br/>
                         Build #: <b>${env.BUILD_NUMBER}</b><br/>
                         Status: <b>FAILED</b></p>
                         <p>Details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'janhvidahake2001@gmail.com'
            )
        }
    }
}
