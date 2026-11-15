
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

stage('Docker Deploy') {
    steps {
        echo 'Deploying Docker container...'

        bat '''
            docker stop dotnetcicd-container 2>NUL || exit /b 0
            docker rm dotnetcicd-container 2>NUL || exit /b 0

            docker run -d ^
                --name dotnetcicd-container ^
                -p 5000:8080 ^
                dotnetcicd:%BUILD_NUMBER%
        '''
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
