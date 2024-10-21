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
        stage('Static Analysis (SAST)') {
            steps {
                sh 'echo Running SAST...'
                // Установите зависимости SAST, если они требуются
                // Для примера, это может быть установка инструмента, например:
                // sh 'npm install -g some-sast-tool'
                // или любой другой SAST инструмент, который вы используете.
            }
        }
        stage('TruffleHog') {
            steps {
                sh 'echo Running TruffleHog...'
                // Установка TruffleHog
                sh 'pip install truffleHog'  // Убедитесь, что pip установлен в вашем агенте
                // Запуск TruffleHog
                sh 'trufflehog --json . > trufflehog_report.json'
            }
            post {
                always {
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
