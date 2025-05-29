pipeline {
    agent any

    stages {
        stage('Clonar Repo') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/johanamileny/docker-basic.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Compilando código...'
            }
        }
        stage('Tests') {
            steps {
                echo 'Ejecutando pruebas...'
            }
        }
    }
}