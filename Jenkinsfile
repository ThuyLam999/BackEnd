pipeline {
    agent any 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: '7d45d4df-5f8a-49e0-af51-c0f629e77c4b', url: 'https://github.com/ThuyLam999/BackEnd.git'
            }
        }
        
        stage('Restore packages'){
            steps{
                bat "dotnet restore BackendAPI/BackendAPI.csproj"
            }
        }

        stage('Clean'){
            steps{
                bat "dotnet clean BackendAPI/BackendAPI.csproj"
            }
        }

        stage('Build'){
            steps{
                bat "dotnet build BackendAPI/BackendAPI.csproj --configuration Release"
            }
        }

        stage('Test: Unit Test'){
            steps {
                echo 'Test: Unit Test'
            }
        }
       
        stage('Test: Integration Test'){
            steps {
                 echo 'Test: Integration Test'
            }
        }

        stage('Build Docker') {
            steps{
                bat '''cd BackendAPI
            docker rmi -f docker_backendAPI_test:1.0
            docker build -t docker_backendAPI_test:1.0 .'''
            }
        }

        stage('Run') {
            steps{
                bat '''docker rm -f docker_backendAPI_test
            docker run --name docker_backendAPI_test -d -p 7489:80 docker_backendAPI_test:1.0'''
            }
        }

        stage('Cleanup') {
            steps {
                deleteDir()
            }
        }
    }
    post {
        success {
            mail bcc: '', body: 'Thông báo kết quả build', cc: '', from: '', replyTo: '', subject: 'Test Run SUCCESSFUL', to: 'thanhthuyyasou234@gmail.com'
        }  

        failure {
            mail bcc: '', body: 'Thông báo kết quả build', cc: '', from: '', replyTo: '', subject: 'Test Run FAILED', to: 'thanhthuyyasou234@gmail.com'
        }
    }
}
