pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий
                git url: 'https://github.com/Alperd24/Alperd24', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                // Сборка проекта (пример для Node.js)
                echo 'Building the application...'
                sh 'npm install' // или другие команды для вашей сборки
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
        
        stage('Static Analysis with AppScreener') {
            steps {
                echo 'Running AppScreener...'
                // Установка и запуск AppScreener
                sh 'curl -sSL https://get.app-screener.com | bash'
                sh 'appscreener check .'
            }
        }

        stage('Secret Detection with TruffleHog') {
            steps {
                echo 'Running TruffleHog...'
                // Установка TruffleHog
                sh 'pip install truffleHog'
                // Запуск TruffleHog
                sh 'trufflehog --json . > trufflehog_report.json'
            }

            post {
                always {
                    // Сохранение отчета как артефакта
                    archiveArtifacts artifacts: 'trufflehog_report.json', fingerprint: true
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Команды для развертывания вашего приложения
                // Например, scp или запуск Docker-контейнера
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
