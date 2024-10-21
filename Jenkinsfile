pipeline {
    agent any 
    stages {
        stage('Install DefectDojo') {
            steps {
                script {
                    sh '''
                        echo "Installing DefectDojo..."
                        # Установка Docker, если он не установлен
                        # Убедитесь, что ваше окружение поддерживает Docker
                        if ! command -v docker &> /dev/null
                        then
                            echo "Docker could not be found. Installing..."
                            # Например, команды для установки Docker
                            # sudo apt-get update
                            # sudo apt-get install -y docker.io
                        fi

                        # Клонирование DefectDojo
                        git clone https://github.com/DefectDojo/django-DefectDojo.git
                        cd django-DefectDojo
                        # Запуск DefectDojo
                        docker-compose up -d
                    '''
                }
            }
        }

        stage('Install Vault') {
            steps {
                script {
                    sh '''
                        echo "Installing Vault..."
                        # Установка Vault
                        # Убедитесь, что ваше окружение поддерживает пакеты
                        if ! command -v vault &> /dev/null
                        then
                            echo "Vault could not be found. Installing..."
                            # команды для установки Vault
                            # Например для Ubuntu:
                            wget https://releases.hashicorp.com/vault/1.9.1/vault_1.9.1_linux_amd64.zip
                            unzip vault_1.9.1_linux_amd64.zip
                            sudo mv vault /usr/local/bin/
                        fi

                        # Запуск Vault
                        vault server -config=/path/to/config.hcl
                    '''
                }
            }
        }

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
