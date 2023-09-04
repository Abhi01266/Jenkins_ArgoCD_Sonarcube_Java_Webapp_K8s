pipeline {
  agent {
    github {
    stage('Update Deployment File') {
        environment {
            GIT_REPO_NAME = "Jenkins_ArgoCD_Sonarcube_Java_Webapp_K8s"
            GIT_USER_NAME = "abhibondar"
        }
        steps {
            withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                sh '''
                    git config user.email "abhibondar01@gmail.com"
                    git config user.name "Abhijeet Bondar"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i "s/replaceImageTag/${BUILD_NUMBER}/g" manifests/deployment.yml
                    git add manifests/deployment.yml
                    git add target/
                    git commit -m "Update image version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
            }
        }
    }
  }
}
