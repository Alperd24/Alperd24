pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Alperd24/Alperd24', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y python3-pip python3-venv'
                    sh 'python3 -m venv venv'
                    sh '. venv/bin/activate'  // Изменено на точку вместо source
                    sh 'pip install -r requirements.txt'  // Предположим, у вас есть requirements.txt
                }
            }
        }
        stage('Run TruffleHog') {
            steps {
                script {
                    sh '. venv/bin/activate'
                    sh 'trufflehog --json . > trufflehog_report.json'
                }
            }
        }
        stage('Install AppScreener') {
            steps {
                script {
                    sh 'curl -sSL https://get.app-screener.com | bash'
                }
            }
        }
        stage('Run AppScreener') {
            steps {
                script {
                    sh '. venv/bin/activate'
                    sh 'appscreener check .'
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'trufflehog_report.json', allowEmptyArchive: true
            cleanup()
        }
    }
}
