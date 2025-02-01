pipeline {
    agent any  // Задаем, что pipeline может быть выполнен на любом агенте

    stages {
        stage('Clone repository') {
            steps {
                script {
                    // Переход в рабочую директорию
                    dir('/opt/jenkins_agent/workspace/MoleculeTestPipline') {
                        // Проверка, существует ли директория ansible_role_unbound
                        if (!fileExists("ansible_role_unbound")) {
                            // Если директория не существует, клонируем репозиторий
                            git url: 'https://github.com/drumspb/ansible_role_unbound', branch: 'main'
                        } else {
                            // Если директория существует, просто обновляем её
                            dir('ansible_role_unbound') {
                                sh 'git pull'
                            }
                        }
                    }
                }
            }
        }

        stage('Run Molecule tests') {
            steps {
                script {
                    // Переход в директорию ansible_role_unbound и запуск тестов
                    dir('/opt/jenkins_agent/workspace/MoleculeTestFreestyle/ansible_role_unbound') {
                        // Установка зависимостей и запуск тестов
                        sh 'molecule test'
                    }
                }
            }
        }
    }

    post {
        always {
            // Очищаем рабочую директорию, если необходимо
            cleanWs()
        }
    }
}