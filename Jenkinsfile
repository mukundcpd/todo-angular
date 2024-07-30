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
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Deploy') {
            steps {
                // Add your deployment steps here
                // For example, copying build files to a server
                sh 'scp -r dist/todo-angular/* user@server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'dist/your-app/**/*', allowEmptyArchive: true
            junit 'path/to/your/test-results.xml'
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
