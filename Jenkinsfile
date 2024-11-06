pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = 'github'  // Nama kredensial GitHub yang telah diset di Jenkins
    }

    stages {
        stage('Clone repository') {
            steps {
                // Clone repository dengan SCM
                checkout scm
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    // Menggunakan kredensial GitHub (username dan password)
                    withCredentials([usernamePassword(credentialsId: env.GIT_CREDENTIALS, 
                                                      usernameVariable: 'GIT_USERNAME', 
                                                      passwordVariable: 'GIT_PASSWORD')]) {
                        // Mengonfigurasi Git user untuk commit
                        sh "git config user.email jkasyqy@gmail.com"
                        sh "git config user.name Vicky112"
                        
                        // Menampilkan file Deployment.yaml sebelum modifikasi
                        sh "cat Deployment.yaml"
                        
                        // Mengganti tag image di Deployment.yaml dengan BUILD_NUMBER
                        sh "sed -i 's|vikiardian/react-app.*|vikiardian/react-app:${BUILDTAG}|g' Deployment.yaml"
                        
                        // Menampilkan file Deployment.yaml setelah modifikasi
                        sh "cat Deployment.yaml"
                        
                        // Menambahkan perubahan ke git
                        sh "git add ."
                        
                        // Commit perubahan
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        
                        // Push perubahan ke GitHub
                        sh """
                            git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/react-app-manifest.git HEAD:main
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            // Untuk log atau pembersihan setelah build selesai
            echo "Pipeline complete"
        }

        success {
            // Menangani keadaan jika pipeline berhasil
            echo "Pipeline completed successfully!"
        }

        failure {
            // Menangani keadaan jika pipeline gagal
            echo "Pipeline failed!"
        }
    }
}
