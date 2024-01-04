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
        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
         }
        stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "Vikta96"
                    git config --global user.email "anandakbm96@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github credentials', gitToolName: 'reddit-git')]) {
                    sh "git push https://github.com/Vikta96/Reddit-APP-CD.git main"
                }
            }
         }
    }
}
