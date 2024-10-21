pipeline {
    agent any 
    stages {
        stage('Clone') {
            steps {
                script {
                    git url: 'https://github.com/Alperd24/Alperd24', branch: 'main'
                }
            }
        }

        stage('Install gitleaks') {
            steps {
                script {
                    sh 'echo Installing gitleaks...'
                    sh 'sudo apt install gitleaks'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'echo Building the application...'
                // sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'echo Running tests...'
                    sh 'mvn test'
                }
            }
        }
        stage('Security Check with gitleaks') {
            steps {
                script {
                    sh 'echo Running gitleaks...'
                    sh 'gitleaks detect --source=./' 
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo Deploying application...'
                // sh 'scp target/*.war user@server:/path/to/deploy'
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
