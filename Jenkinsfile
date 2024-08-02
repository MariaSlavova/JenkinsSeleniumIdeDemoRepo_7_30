pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/MariaSlavova/JenkinsSeleniumIdeDemoRepo_7_30'
            }
        }
        stage('Setup .Net core') {
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -L -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe 
                echo Installing dotnet-sdk-6.0.132-win-x64.exe
                dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
                '''
            }
        }
        stage('Restoring nuget packages') {
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}