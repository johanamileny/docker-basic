pipeline {
    agent any
    
    tools {
        nodejs 'node-20.19.2'
        dotnetsdk 'dotnet-9.0.203'
    }
   
    environment {
       
        PATH = "${env.PATH}:/home/server3/.dotnet/tools"
        SONARQUBE_URL = 'http://192.168.1.57:9000'
        SONARQUBE_TOKEN = 'squ_612aaa1af4d032e732923ac391ebd8a24a3a56d2'
    
}

    stages {
        stage('Check versions') {
            steps {
                echo 'Node.js version:' 
                sh 'node -v'
                echo 'NPM version:'
                sh 'npm -v'
                echo 'Dotnet version:'
                sh 'dotnet --version'
            }
        }

        stage('Backend - Restore') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Restoring dependencies...'
                    sh 'dotnet restore'
                }
            }
        }

        stage('Backend - Static Analysis') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running static analysis...'
                    sh '''
                        dotnet sonarscanner begin /k:"Docker-Basic" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="squ_612aaa1af4d032e732923ac391ebd8a24a3a56d2"
                        dotnet build
                        dotnet sonarscanner end /d:sonar.login="squ_612aaa1af4d032e732923ac391ebd8a24a3a56d2"
                    '''
                }
            }
        }

        stage('Backend - Test') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running tests...'
                    sh 'dotnet test --no-build --verbosity normal'
                }
            }
        }

        stage('Backend - Build') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Building the project...'
                    sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }

        stage('Backend - Publish') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Publishing the project...'
                    sh 'dotnet publish --configuration Release --no-build -o ./publish'
                }
            }
        }

        stage('Frontend - Install') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Frontend - Test') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Running tests...'
                    sh 'npm test'
                }
            }
        }

        stage('Backend - Code Coverage') {
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running code coverage...'
                    sh 'dotnet test --collect:"XPlat Code Coverage" --no-build --verbosity normal'
                }
            }
        }

        stage('Frontend - Build') {
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Building the project...'
                    sh 'npm run build'
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages.'
        }
    }
}


