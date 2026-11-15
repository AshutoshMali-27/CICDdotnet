
pipeline {
    agent any

    stages {

        stage('Restore') {
            steps {
                echo 'Restoring NuGet packages...'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building solution...'
                bat 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated unit tests...'
                bat 'dotnet test DotNetCICD.Tests\\DotNetCICD.Tests.csproj --configuration Release --no-build --no-restore'
            }
        }
stage('Docker Build') {
    steps {
        echo 'Building Docker image...'
        bat 'docker build -t dotnetcicd:%BUILD_NUMBER% .'
    }
}
    }

    post {
        success {
            echo 'CI Pipeline completed successfully!'
        }

        failure {
            echo 'CI Pipeline failed. Check the console output.'
        }
    }
}
