pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Alperd24/Alperd24'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'sudo apt-get update'
                sh 'sudo apt-get install -y python3-full python3-venv'
                sh 'python3 -m venv venv'
                sh '''
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        
        stage('Run TruffleHog') {
            steps {
                sh '''
                . venv/bin/activate
                trufflehog --json . > trufflehog_report.json || echo "TruffleHog run failed"
                '''
            }
        }
        
        stage('Install AppScreener') {
            steps {
                sh '''
                curl -sSL https://get.app-screener.com | bash
                '''
            }
        }

        stage('Run AppScreener') {
            steps {
                sh '''
                appscreener check . || echo "AppScreener run failed"
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'trufflehog_report.json', allowEmptyArchive: true
        }
    }
}
