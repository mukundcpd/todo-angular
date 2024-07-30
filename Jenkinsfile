pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // The name you gave NodeJS installation
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                git credentialsId: 'jenkin-cicd-angular1', url: 'https://github.com/mukundcpd/todo-angular.git'
                echo 'Checkout complete.'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
                echo 'Dependencies installed.'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
                echo 'Tests completed.'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'npm run build --prod'
                echo 'Build complete.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Add your deployment steps here
                // For example, copying build files to a server
                // sh 'scp -r dist/todo-angular/* user@server:/path/to/deploy'
                echo 'Deployment complete.'
            }
        }
    }

    post {
        always {
            echo 'Archiving artifacts...'
            archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
            echo 'Artifacts archived.'
            echo 'Publishing test results...'
            junit '**/TEST-*.xml'
            echo 'Test results published.'
        }
        success {
            mail to: 'team@example.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' succeeded."
            echo 'Success email sent.'
        }
        failure {
            mail to: 'team@example.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed.",
                 body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed."
            echo 'Failure email sent.'
        }
    }
}
