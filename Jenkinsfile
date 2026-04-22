pipeline {
    agent any // Запускать на любом свободном исполнителе (воркере)

    stages {
        stage('Checkout') {
            steps {
                // Jenkins сам скачивает код из репозитория, 
                // если этот билд запущен через Pipeline из SCM
                checkout scm
            }
        }

        stage('Build & Deploy') {
            steps {
                script {
                    // Используем docker-compose для пересборки и запуска
                    // --build заставит Docker пересобрать образ, если код изменился
                    // -d запустит всё в фоновом режиме
                    sh 'docker compose up --build -d'
                }
            }
        }

        stage('Cleanup') {
            steps {
                // Очистка неиспользуемых "висячих" образов, 
                // чтобы не забивать память инстанса AWS
                sh 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo 'Приложение успешно развернуто!'
        }
        failure {
            echo 'Что-то пошло не так. Проверь логи сборки.'
        }
    }
}
