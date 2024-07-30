pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // The name you gave NodeJS installation
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'jenkin-cicd-angular1', url: 'https://github.com/mukundcpd/todo-angular.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build --prod'
            }
        }

        stage('Deploy') {
            steps {
                // Add your deployment steps here
                // For example, copying build files to a server
                // If you are using a Windows server for deployment, you can use robocopy or xcopy
                bat 'robocopy dist\\todo-angular\\ \\path\\to\\deploy /E /Z /COPYALL /R:0 /W:0'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'dist\\todo-angular\\**\\*', allowEmptyArchive: true
            // Update the path to your test results if you have them
            junit 'path\\to\\your\\test-results.xml'
        }
        success {
            mail to: 'team@example.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' succeeded."
        }
        failure {
            mail to: 'team@example.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed."
        }
    }
}
