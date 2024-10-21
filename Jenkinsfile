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
        stage('Static Analysis with AppScreener') {
            steps {
                sh 'echo Running AppScreener...'
                // Установка AppScreener
                sh 'curl -sSL https://get.app-screener.com | bash'
                // Запуск анализа
                sh 'appscreener check .'
            }
        }
        stage('Secret Detection with TruffleHog') {
            steps {
                sh 'echo Running TruffleHog...'
                // Установка TruffleHog
                sh 'pip install truffleHog'
                // Запуск TruffleHog для поиска секретов
                sh 'trufflehog --json . > trufflehog_report.json'
            }
            post {
                always {
                    // Сохранение отчета как артефакт
                    archiveArtifacts artifacts: 'trufflehog_report.json', allowEmptyArchive: true
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
