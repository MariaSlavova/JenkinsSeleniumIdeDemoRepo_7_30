pipeline {
    agent any 

    stages {
        stage('Checkout code') {
            //checkout the repository
            steps {
                git branch: 'main', url:'https://github.com/MariaSlavova/JenkinsSeleniumIdeDemoRepo_7_30'
               
               }   
            }
            stage('St up .Net core') {
            //checkout the repository
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -l -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe 
                echo installing dotnet-sdk-6.0.132-win-x64.exe
                dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
                '''
               
               }   
            }
            stage('Restoring nuget packages') {
            //checkout the repository
            steps {
                bat 'dotnet restore SeleniumIde.sln'
                
               }   
            }
            stage('Build') {
            //checkout the repository
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
                
               } 
            stage('Run Tests') {
            //checkout the repository
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResiults.trx"'
            } 
        }
    }
    
    post {
        always {
            archiveSrtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResiultsFile: '**/TestResults/*.trx'
            ])
        }
    }
} 