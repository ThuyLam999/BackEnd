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

        stage('Docker Build and Tag') {
            steps {    
                bat 'docker build --tag lptest999/docker_backendAPI_test:1.0.0 .' 
                bat 'docker tag docker_backendAPI_test lptest999/docker_backendAPI_test:1.0.0'               
            }
        }
     
        stage('Publish image to Docker Hub') {
            steps {
                withDockerRegistry([ credentialsId: "dockerhubid", url: "https://registry.hub.docker.com" ]) {
                    bat  'docker push lptest999/docker_backendAPI_test:1.0.0'
                }       
            }
        }
     
        stage('Run Docker container on Jenkins Agent') {
            steps 
			{
                bat "docker run -d -p 8003:8080 lptest999/docker_backendAPI_test:1.0.0"
            }
        }

        stage('Run Docker container on remote hosts') {        
            steps {
                bat "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 lptest999/docker_backendAPI_test:1.0.0"
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
