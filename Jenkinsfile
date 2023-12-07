pipeline {
    agent any

    environment {
        LW_ACCESS_TOKEN = credentials('LW_ACCESS_TOKEN')
        LW_ACCOUNT_NAME = credentials('LW_ACCOUNT_NAME')
        LW_SCANNER_SAVE_RESULTS=true
    }

    parameters {
        string (name: 'IMAGE_NAME',
                description: "Specify Image Name",
                defaultValue: '')
        string (name: 'IMAGE_TAG',
                description: "Specify Image Tag",
                defaultValue: '')
    }

    stages {
        stage('Pull') {
            steps {
                echo 'Pulling image ...'
                sh "docker pull ${IMAGE_NAME}:${IMAGE_TAG}" //Pull the image to scan
            }
        }
        stage('Scan') {
            steps {
                echo 'Scanning image ...'
                sh "curl -L https://github.com/lacework/lacework-vulnerability-scanner/releases/latest/download/lw-scanner-linux-amd64 -o lw-scanner"
                sh "chmod +x lw-scanner"
                sh "./lw-scanner image evaluate ${IMAGE_NAME} ${IMAGE_TAG} --build-id ${BUILD_ID}"
            }
        }
    }
}
