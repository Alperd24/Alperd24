pipeline {
    agent any 

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий
                git url: 'https://github.com/Alperd24/Alperd24', branch: 'main'
            }
        }
        
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Обновление пакетов и установка python3-venv
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y python3-pip python3-venv'

                    // Создание виртуального окружения
                    sh 'python3 -m venv venv'
                    // Активация виртуального окружения и установка зависимостей
                    sh 'source venv/bin/activate && pip install --upgrade pip'
                }
            }
        }

        stage('Run TruffleHog') {
            steps {
                script {
                    // Активация виртуального окружения и установка TruffleHog
                    sh 'source venv/bin/activate && pip install truffleHog'
                    // Запуск TruffleHog
                    sh 'source venv/bin/activate && trufflehog --json . > trufflehog_report.json'
                }
            }
        }

        stage('Install AppScreener') {
            steps {
                script {
                    // Установка и запуск AppScreener
                    sh 'curl -sSL https://get.app-screener.com | bash'
                }
            }
        }

        stage('Run AppScreener') {
            steps {
                script {
                    // Запуск AppScreener
                    sh 'appscreener check .'
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                sh 'rm trufflehog_report.json' // Удаление отчета (по желанию)
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
