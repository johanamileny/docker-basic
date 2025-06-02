pipeline {
    agent any

    tools {
        dotnetsdk 'dotnet-9'
        nodejs 'node-22'
    }
    environment {
        DOTNET_ROOT = "${env.PATH}:${tool 'dotnet-9'}/bin"
        PATH = "${env.PATH}:${tool 'node-22'}/bin"
    }

    stages {
        stage('Check versions'){
            steps {
                script {
                    echo 'Node.js version:' 
                    sh 'node -v'
                    echo 'NPM version:'
                    sh 'npm -v'
                    echo 'Dotnet version:'
                    sh 'dotnet --version'
                }
            }
        }
        stage('Backend - Restore'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Restoring dependencies...'
                    sh 'dotnet restore'
                }
            }
        }
        stage('Backend - Test'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running tests...'
                    sh 'dotnet test --no-build --verbosity normal'
                }
            }
        }
        stage('Backend - Build'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Building the project...'
                    sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }  
        stage('Backend - Publish'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Publishing the project...'
                    sh 'dotnet publish --configuration Release --no-build -o ./publish'
                }
            }
            
        } 
        stage('Frontend - Install'){
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }
    }
    
    }

    post {
        always {
            echo 'This will always run after the stages.'
        }
    }


