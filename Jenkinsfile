pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE', defaultValue: '', description: 'The Docker image tag to update in the manifest')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Update Kubernetes Manifest') {
            steps {
                script {
                    echo "Updating Kubernetes manifest with image: ${params.DOCKER_IMAGE}"
                    sh """
                    sed -i 's|image: .*|image: ${params.DOCKER_IMAGE}|g' Deployment.yaml
                    """
                    echo "Kubernetes manifest updated."
                }
            }
        }

        stage('Commit and Push Manifest Changes') {
            steps {
                script {
                    echo "Committing and pushing updated manifest to Git."
                    sh """
                    git config --global user.name "Vicky"
                    git config --global user.email "jkasyqy@gmail.com"
                    git add .
                    git commit -m "Update image tag to ${params.DOCKER_IMAGE}"
                    git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/react-app-manifest.git HEAD:main
                    """
                }
            }
        }
    }
}
