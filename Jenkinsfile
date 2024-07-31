pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // The name you gave NodeJS installation
    }

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials-id' // Update with your Docker credentials ID
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
                echo 'Stashing build artifacts...'
                stash includes: 'dist/**/*', name: 'build-artifacts'
                echo 'Build artifacts stashed.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Unstashing build artifacts...'
                unstash 'build-artifacts'
                echo 'Build artifacts unstashed.'
                echo 'Deploying application...'
                // Add your deployment steps here
                // For example, copying build files to a server
                // sh 'scp -r dist/todo-angular/* user@server:/path/to/deploy'
                echo 'Deployment complete.'
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    echo 'Unstashing build artifacts for Docker build...'
                    unstash 'build-artifacts'
                    echo 'Building Docker image...'
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def appImage = docker.build('mukuncpd/todo-angular', '--build-arg CACHEBUST=$(date +%s) .')
                        appImage.push('latest')
                    }
                    echo 'Docker image built and pushed.'
                }
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
