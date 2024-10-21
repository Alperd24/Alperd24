pipeline {
    agent any 

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий
                git url: 'https://github.com/Alperd24/Alperd24', branch: 'main'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    // Установите pip и TruffleHog
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y python3-pip'
                    sh 'pip3 install truffleHog'
                }
            }
        }

        stage('Run TruffleHog') {
            steps {
                script {
                    sh 'trufflehog --json . > trufflehog_report.json'
                }
            }
        }

        stage('Install AppScreener') {
            steps {
                script {
                    // Установите AppScreener (для этого используется curl)
                    sh 'curl -sSL https://get.app-screener.com | bash'
                }
            }
        }

        stage('Run AppScreener') {
            steps {
                script {
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
