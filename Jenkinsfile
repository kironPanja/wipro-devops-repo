pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/kironPanja/wipro-devops-repo.git'
        BRANCH = 'main'
        WORK_DIR = 'wipro-devops-repo'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('List HTML Files') {
            steps {
                script {
                    def htmlFiles = sh(script: "find . -name '*.html'", returnStdout: true).trim()
                    echo "HTML Files Found:\n${htmlFiles}"
                }
            }
        }

        stage('Validate HTML') {
            steps {
                // Optional: Use a tool like htmlhint or tidy for validation
                echo 'Validating HTML files...'
                // Example: sh 'htmlhint .'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.html', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
