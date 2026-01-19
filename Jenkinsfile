pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // This is the line you mentioned, wrapped properly in a pipeline stage
                git branch: 'main', 
                    url: 'https://github.com/kironPanja/wipro-devops-repo.git', 
                    credentialsId: 'github-creds'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                // Add build commands here, e.g. npm install, mvn package, etc.
            }
        }

        stage('Deploy to Nginx') {
            steps {
                echo 'Deploying files to Nginx...'
                sh '''
                sudo cp -r * /usr/share/nginx/html/
                sudo systemctl restart nginx
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check logs.'
        }
    }
}
