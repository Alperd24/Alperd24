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

        stage('Install nexus') {
            steps {
                script {
                    sh 'echo Installing nexus...'
                    sh 'sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz'
                    sh 'sudo tar -xzvf latest-unix.tar.gz'
                    sh 'sudo systemctl start nexus'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'echo Building the application...'
                // ваши команды сборки, например:
                // sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'echo Running tests...'
                // ваши команды тестирования, например:
                // sh 'mvn test'
            }
        }
        stage('Security Check with gitleaks') {
            steps {
                script {
                    sh 'echo Running gitleaks...'
                    sh 'gitleaks detect --source=./'  // Выполните проверку на наличие утечек
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo Deploying application...'
                // ваши команды развертывания, например:
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
