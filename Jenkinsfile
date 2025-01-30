pipeline {
    agent any

    environment {
        SOLUTION_FILE = 'jenkins_publish_test.sln'  // Solution file name
        PROJECT_FOLDER = 'jenkins_publish_test'     // Project folder
        BUILD_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Jenkins_test_publish_master\\publish'
        DEPLOY_PATH = 'C:\\inetpub\\wwwroot\\jenkins_publish_test'  // IIS Deployment Path
        MSBUILD_PATH = 'C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Enterprise\\MSBuild\\Current\\Bin\\amd64'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/AyahAbuzaid/jenkins_publish_test.git'
            }
        }

        stage('Restore NuGet Packages') {
            steps {
                bat '"C:\\NuGet\\nuget.exe" restore "%SOLUTION_FILE%"'
            }
        }

        stage('Build Solution') {
            steps {
                withEnv(["PATH=${MSBUILD_PATH};%PATH%"]) {
                    bat """
                        MSBuild.exe "%SOLUTION_FILE%" ^
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
                    bat """
                        xcopy /E /Y /I "${BUILD_DIR}\\${PROJECT_FOLDER}" "${DEPLOY_PATH}"
                        iisreset
                    """
                }
            }
        }
    }
}
