node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email jkasyqy@gmail.com"
                        sh "git config user.name Vicky112"
                        //sh "git switch master"
                        sh "cat Deployment.yaml"
                        sh "sed -i 's|vikiardian/react-app.*|vikiardian/react-app:${DOCKERTAG}|g' Deployment.yaml"
                        sh "cat Deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/react-app-manifest.git HEAD:main"
      }
    }
  }
}
}

