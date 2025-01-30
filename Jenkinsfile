pipeline {
    agent any

    environment {
        SOLUTION_FILE = 'jenkins_publish_test.sln'
        PROJECT_FOLDER = 'jenkins_publish_test'
        BUILD_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\jenkins_publish_test\\publish'
        DEPLOY_PATH = 'C:\\inetpub\\wwwroot\\jenkins_publish_test'
        MSBUILD_PATH = 'C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin'
        CMD_PATH = 'C:\\Windows\\System32\\cmd.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/AyahAbuzaid/jenkins_publish_test.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat '"C:\\Windows\\System32\\cmd.exe" /c "C:\\NuGet\\nuget.exe restore "%SOLUTION_FILE%"'
            }
        }

        stage('Build Solution') {
            steps {
                withEnv(["PATH=${MSBUILD_PATH};%PATH%"]) {
                    bat ""
                        "${CMD_PATH}" /c "${MSBUILD_PATH}\\MSBuild.exe" "%SOLUTION_FILE%" ^
                        /p:Configuration=Release ^
                        /p:Platform="Any CPU" ^
                        /p:DeployOnBuild=true ^
                        /p:WebPublishMethod=FileSystem ^
                        /p:PublishUrl="${BUILD_DIR}"
                    ""
                    
                }
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    bat """
                        "${CMD_PATH}" /c xcopy /E /Y /I "${BUILD_DIR}\\${PROJECT_FOLDER}" "${DEPLOY_PATH}"
                        "${CMD_PATH}" /c iisreset
                    """
                }
            }
        }
    }
}
