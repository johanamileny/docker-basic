pipeline {
    agent any

    tools {
        dotnet 'dotnet-9.0.203'
        nodejs 'node-20.19.2'
    }

    environment {
        CI = 'true'
    }

    stages {
        stage('Clonar Repo') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/johanmilkey/docker-basic.git'
            }
        }

        stage('Verificar Versiones') {
            steps {
                sh 'dotnet --version'
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Build Backend') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Compilando Backend (.NET)...'
                    sh 'dotnet build'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Instalando dependencias Frontend...'
                    sh 'npm install'
                    echo 'Construyendo Frontend (Remix)...'
                    sh 'npm run build'
                }
            }
        }

        stage('Tests Backend') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Ejecutando pruebas Backend (.NET)...'
                    sh 'dotnet test'
                }
            }
        }

        stage('Tests Frontend') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Ejecutando pruebas Frontend...'
                    sh 'npm test'
                }
            }
        }
    }
}