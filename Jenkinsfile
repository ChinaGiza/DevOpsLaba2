pipeline {
    agent any
    
    parameters {
        string(name: 'STUDENT_NAME', defaultValue: 'Chingiz Mahmutov', description: 'Имя студента')
        string(name: 'PORT', defaultValue: '8091', description: 'Порт')
    }
    
    stages {
        stage('Удаляем старые контейнеры и образы') {
            steps {
                script {
                    // Останавливаем и удаляем контейнер с нужным именем, если он есть
                    sh "docker ps -a -q --filter name=hello-mahmutov-container | xargs -r docker rm -f"
                    // Удаляем образ с нужным именем, если он есть
                    sh "docker images -q student-mahmutov-app | xargs -r docker rmi -f"
                }
            }
        }
        stage('Выгружаем код из репозитория') {
            steps {
                git 'https://github.com/ChinaGiza/DevOpsLaba2.git'
            }
        }
        stage('Собираем docker image') {
            steps {
                script {
                    dockerImage = docker.build("student-mahmutov-app")
                }
            }
        }
        stage('Запускаем тесты в докере') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'python -m unittest test_app.py'
                    }
                }
            }
        }
        stage('Запускаем докер контейнер') {
            steps {
                script {
                    sh "docker run -d --name hello-mahmutov-container -p ${params.PORT}:${params.PORT} -e STUDENT_NAME='${params.STUDENT_NAME}' -e PORT=${params.PORT} student-mahmutov-app"
                }
            }
        }
    }
}
