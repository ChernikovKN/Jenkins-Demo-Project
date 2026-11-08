pipeline {
    agent any

    parameters {
        string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'Версия приложения для сборки')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'stage', 'prod'], description: 'Куда будем деплоить?')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Выполнять ли тесты?')
    }

    environment {
        BUILD_USER = "${env.BUILD_USER_ID ?: 'Anonymous'}"
        SERVER_URL = "http://server-${params.ENVIRONMENT}.company.local"
    }

    stages {
        stage('Подготовка') {
            steps {
                echo "Сборку запустил: ${env.BUILD_USER}"
                echo "Сборка версии: ${params.APP_VERSION} для окружения: ${params.ENVIRONMENT}"
            }
        }
        stage('Сборка') {
            steps {
                echo "Компиляция приложения версии ${params.APP_VERSION}..."
            }
        }
        stage('Тестирование') {
            when {
                expression { return params.RUN_TESTS == true }
            }
            steps {
                echo "Запуск тестов для версии ${params.APP_VERSION}..."
            }
        }
        stage('Развертывание') {
            steps {
                echo "Деплой на сервер ${env.SERVER_URL}"
                script {
                    if (params.ENVIRONMENT == 'prod') {
                        input message: "Подтвердите деплой в production!", ok: "Деплоим"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Сборка версии ${params.APP_VERSION} завершена."
        }
    }
}
