pipeline {
    agent any
    environment {
        GIT_REPO_NAME = "Reddit-APP-CD"
        GIT_USER_NAME = "Vikta96"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', url: 'https://github.com/Vikta96/Reddit-APP-CD.git'
            }
        }
        stage('Update Deployment File') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                        // Determine the image name dynamically based on your versioning strategy
                        NEW_IMAGE_NAME = "vikta96/reddit-app-ci:latest"

                        // Replace the image name in the deployment.yaml file
                        sh "sed -i 's|image: .*|image: $NEW_IMAGE_NAME|' deployment.yml"

                        // Git commands to stage, commit, and push the changes
                        sh 'git add deployment.yml'
                        sh "git commit -m 'Update deployment image to $NEW_IMAGE_NAME'"
                        sh "git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main"
                    }    
                }
            }
        }
    }
}
