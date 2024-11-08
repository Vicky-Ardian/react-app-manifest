pipeline {
    agent any
    environment {
        GITHUB_CREDENTIALS = credentials('github')
    }
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
                    // Menggunakan kredensial GitHub untuk melakukan push
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                            // Konfigurasi email dan username untuk git commit
                            sh "git config user.email 'jkasyqy@gmail.com'"
                            sh "git config user.name 'Vicky112'"

                            // Menampilkan isi file deployment.yaml sebelum perubahan
                            sh "cat Deployment.yaml"

                            // Mengganti image tag pada deployment.yaml dengan tag Docker baru
                            sh "sed -i 's|vikiardian/react-app.*|vikiardian/react-app:${DOCKERTAG}|g' Deployment.yaml"

                            // Menampilkan isi file deployment.yaml setelah perubahan
                            sh "cat Deployment.yaml"

                            // Menambahkan perubahan, commit dan push ke GitHub
                            sh "git add ."
                            sh "git commit -m 'Update manifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/react-app-manifest.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
