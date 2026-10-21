
pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *')
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Compile / Validate') {
            steps {
                bat 'python -m py_compile app.py'
            }
        }

        stage('Test') {
            steps {
                bat 'python -m unittest discover -s tests -v'
            }
        }

        stage('Package') {
            steps {
                bat 'powershell -NoProfile -Command "Compress-Archive -Path app.py,tests -DestinationPath app-package.zip -Force"'
                archiveArtifacts artifacts: 'app-package.zip', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Python CI/CD pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
