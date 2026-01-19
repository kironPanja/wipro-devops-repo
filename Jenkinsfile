pipeline {
    agent any

    environment {
        LOCAL_DIR = 'C:/Users/kiron/Desktop/Code-Commit-New' // Change to your actual local path
        REPO_DIR = 'wipro-devops-repo'
        GIT_URL = 'https://github.com/kironPanja/wipro-devops-repo.git'
        GIT_CREDENTIALS_ID = 'your-jenkins-git-credentials-id' // Set this up in Jenkins
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                echo 'Cleaning workspace...'
                deleteDir()
            }
        }

        stage('Copy Local Files') {
            steps {
                echo "Copying files from ${LOCAL_DIR}..."
                bat "xcopy /E /I /Y \"${LOCAL_DIR}\" \"${REPO_DIR}\""
            }
        }

        stage('Initialize Git Repo') {
            steps {
                dir("${REPO_DIR}") {
                    bat '''
                    git init
                    git remote add origin %GIT_URL%
                    git add .
                    git commit -m "Automated commit from Jenkins"
                    '''
                }
            }
        }

        stage('Push to GitHub') {
            steps {
                dir("${REPO_DIR}") {
                    withCredentials([usernamePassword(credentialsId: "${GIT_CREDENTIALS_ID}", usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        bat '''
                        git config user.name "%GIT_USER%"
                        git config user.email "jenkins@example.com"
                        git push https://%GIT_USER%:%GIT_PASS%@github.com/kironPanja/wipro-devops-repo.git main
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Files successfully pushed to GitHub!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
