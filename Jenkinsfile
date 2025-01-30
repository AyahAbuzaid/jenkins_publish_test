pipeline {
    agent any

    environment {
        SOLUTION_FILE = 'jenkins_publish_test.sln'
        PROJECT_FOLDER = 'jenkins_publish_test'
        BUILD_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\jenkins_publish_test\\publish'
        DEPLOY_PATH = 'C:\\inetpub\\wwwroot\\jenkins_publish_test'
        MSBUILD_PATH = 'C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/AyahAbuzaid/jenkins_publish_test.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                echo "Restoring NuGet Packages"
                bat '"C:\\Windows\\System32\\cmd.exe" /c "C:\\NuGet\\nuget.exe restore %SOLUTION_FILE%"'
            }
        }

        stage('Build Solution') {
            steps {
                withEnv(["PATH=${MSBUILD_PATH};%PATH%"]) {
                    echo "Building Solution"
                    bat """
                        echo "Using MSBuild from: ${MSBUILD_PATH}"
                        call "C:\\Windows\\System32\\cmd.exe" /c "${MSBUILD_PATH}\\MSBuild.exe" "%SOLUTION_FILE%" ^ 
                        /p:Configuration=Release ^ 
                        /p:Platform="Any CPU" ^ 
                        /p:DeployOnBuild=true ^ 
                        /p:WebPublishMethod=FileSystem ^ 
                        /p:PublishUrl="${BUILD_DIR}"
                    """
                }
            }
        }

        stage('Deploy to IIS') {
            steps {
                script {
                    echo "Deploying to IIS"
                    bat """
                        call "C:\\Windows\\System32\\cmd.exe" /c xcopy /E /Y /I "${BUILD_DIR}\\${PROJECT_FOLDER}" "${DEPLOY_PATH}"
                        call "C:\\Windows\\System32\\cmd.exe" /c iisreset
                    """
                }
            }
        }
    }
}
