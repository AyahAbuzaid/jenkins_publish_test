pipeline {
    agent any

    environment {
        SOLUTION_FILE = 'jenkins_publish_test.sln'  // Update with your solution file
        PROJECT_FOLDER = 'jenkins_publish_test'       // Update with your project folder
        BUILD_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Maven Plugin Test Job\\publish'
        DEPLOY_PATH = 'C:\\inetpub\\wwwroot\\jenkins_publish_test'  // IIS Deployment Path
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/AyahAbuzaid/jenkins_publish_test.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat '"C:\\NuGet\\nuget.exe" restore jenkins_publish_test.sln'
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
