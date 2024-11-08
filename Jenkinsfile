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

        stage('Commit and Push Manifest Changes') {
            steps {
                script {
                    // Menggunakan kredensial GitHub untuk melakukan push
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'your-credential-id', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USER')]) {
                            // Konfigurasi email dan username untuk git commit
                            sh "git config user.email 'jkasyqy@gmail.com'"
                            sh "git config user.name 'Vicky112'"

                            // Menampilkan isi file deployment.yaml sebelum perubahan
                            sh "cat Deployment.yaml"

                            // Mengganti image tag pada deployment.yaml dengan tag Docker baru
                            sh "sed -i 's|image: .*|image: ${params.DOCKER_IMAGE}|g' Deployment.yaml"

                            // Menampilkan isi file deployment.yaml setelah perubahan
                            sh "cat Deployment.yaml"

                            // Menambahkan perubahan, commit dan push ke GitHub
                            sh "git add ."
                            sh "git commit -m 'Update manifest: ${env.BUILD_NUMBER}'"
                            sh "git push https://$GIT_USER:$GIT_PASSWORD@github.com/Vicky-Ardian/react-app-manifest.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
