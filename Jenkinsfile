pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Ваша команда сборки, например:
                // sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // Ваша команда тестирования, например:
                // sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Ваша команда развертывания, например:
                // sh 'scp target/*.war user@server:/path/to/deploy'
            }
        }
    }
}
