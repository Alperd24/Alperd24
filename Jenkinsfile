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
                sh 'echo Installing AppScreener...'
                sh 'curl -sSL https://get.app-screener.com | bash'
                sh 'appscreener check .' // Выполнение анализа
            }
        }
        stage('Secret Detection with TruffleHog') {
            steps {
                sh 'echo Installing TruffleHog...'
                sh 'pip install truffleHog' // Установка через pip
                sh 'trufflehog --json . > trufflehog_report.json' // Запуск TruffleHog
            }
            post {
                always {
                    archiveArtifacts artifacts: 'trufflehog_report.json', allowEmptyArchive: true // Сохранение отчета как артефакт
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
