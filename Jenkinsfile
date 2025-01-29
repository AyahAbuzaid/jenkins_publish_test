pipeline {
    agent any

    environment {
        SOLUTION_FILE = 'YourSolution.sln'  // Update with your solution file
        PROJECT_FOLDER = 'YourWebApp'       // Update with your project folder
        BUILD_DIR = '$(Build.ArtifactStagingDirectory)'
        DEPLOY_PATH = 'C:\\inetpub\\wwwroot\\MyWebApp'  // IIS Deployment Path
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your/repo.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat 'nuget restore %SOLUTION_FILE%'
            }
        }

        stage('Build Solution') {
            steps {
                bat '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\MSBuild\\Current\\Bin\\MSBuild.exe" %SOLUTION_FILE% /p:Configuration=Release /p:Platform="Any CPU" /p:DeployOnBuild=true /p:WebPublishMethod=FileSystem /p:PublishUrl=%BUILD_DIR%'
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    def deployCmd = """
                        xcopy "%BUILD_DIR%\\${PROJECT_FOLDER}" "%DEPLOY_PATH%" /E /Y /I
                        iisreset
                    """
                    bat deployCmd
                }
            }
        }
    }
}
