pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Replace with the name you gave NodeJS installation in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git credentialsId: 'jenkin-cicd-angular1', url: 'https://github.com/mukundcpd/todo-angular.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run unit tests
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                // Build the Angular application
                sh 'npm run build --prod'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive the build artifacts
                archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
            }
        }

        stage('Deploy') {
            steps {
                // Add deployment steps here if needed
                // For example, copying build files to a server
                // sh 'scp -r dist/todo-angular/* user@server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            // Archive test results if available
            junit '**/test-results/*.xml'
        }
        success {
            // Send success email or notifications
            mail to: 'team@example.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' succeeded."
        }
        failure {
            // Send failure email or notifications
            mail to: 'team@example.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed.",
                 body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed."
        }
    }
}
