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
                bat "dotnet restore BackEnd/BackendAPI/BackendAPI.sln"
            }
        }

        stage('Clean'){
            steps{
                bat "dotnet clean BackEnd/BackendAPI/BackendAPI.sln"
            }
        }

        stage('Build'){
            steps{
                bat "dotnet build BackEnd/BackendAPI/BackendAPI/BackendAPI.csproj --configuration Release"
            }
        }

        stage('Test: Unit Test'){
            steps {
                echo "Test: Unit Test"
            }
        }
       
        stage('Test: Integration Test'){
            steps {
                 echo "Test: Integration Test"
            }
        }
    }
    post {
        always {
            sshagent(['ssh-remote']) {
                bat 'ssh -o StrictHostKeyChecking=no -l root 45.118.145.149 docker-compose down'
            }
            mail bcc: '', body: 'Thông báo kết quả build', cc: '', from: '', replyTo: '', subject: 'Test', to: 'thanhthuyyasou234@gmail.com'
        }
    }
}
