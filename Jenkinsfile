pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Ensure NodeJS tool is configured
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

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t todo-angular .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker run -p 8000:8000 -d todo-angular'
            }
        }

        stage('Deploy') {
            steps {
                // Example deploy step
                bat 'scp -r dist/todo-angular/* user@server:/path/to/deploy'
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
