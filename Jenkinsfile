pipeline {
    agent any

    environment {
        LOCAL_DIR = 'C:/Users/kiron/Desktop/Code-Commit-New'   // Local path on your machine
        REPO_DIR  = 'wipro-devops-repo'
        GIT_URL   = 'https://github.com/kironPanja/wipro-devops-repo.git'
        GIT_CREDENTIALS_ID = 'your-jenkins-git-credentials-id' // Add in Jenkins credentials
        NGINX_HTML_DIR = '/usr/share/nginx/html'               // Default Nginx HTML folder (Linux)
    }

    stages {
        stage('Copy Local Files to Repo') {
            steps {
                echo "Copying files from local machine to Jenkins workspace..."
                bat "xcopy /E /I /Y \"${LOCAL_DIR}\" \"${REPO_DIR}\""
            }
        }

        stage('Commit & Push to GitHub') {
            steps {
                dir("${REPO_DIR}") {
                    withCredentials([usernamePassword(credentialsId: "${GIT_CREDENTIALS_ID}", usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        bat '''
                        git init
                        git remote add origin https://%GIT_USER%:%GIT_PASS%@github.com/kironPanja/wipro-devops-repo.git
                        git add .
                        git commit -m "Automated commit from Jenkins"
                        git push origin main
                        '''
                    }
                }
            }
        }

        stage('Clone Repo for Deployment') {
            steps {
                git branch: 'main', url: "${GIT_URL}", credentialsId: "${GIT_CREDENTIALS_ID}"
            }
        }

        stage('Deploy to Nginx') {
            steps {
                echo "Deploying files to Nginx HTML folder..."
                sh """
                sudo cp -r * ${NGINX_HTML_DIR}/
                sudo systemctl restart nginx
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully: Files pushed to GitHub and deployed to Nginx.'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
