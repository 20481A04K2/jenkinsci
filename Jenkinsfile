node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                        git config user.email "vij1542044@gmail.com"
                        git config user.name "vamsi"

                        echo "===== Before Update ====="
                        cat deployment.yaml

                        # Update the image tag in the deployment file
                        sed -i 's+sandip525/flaskapp.*+sandip525/flaskapp:${DOCKERTAG}+g' deployment.yaml

                        echo "===== After Update ====="
                        cat deployment.yaml

                        git add .
                        git commit -m 'Done by Jenkins Job changemanifest: ${BUILD_NUMBER}'
                        
                        # Push to your GitOps repo in GitHub (replace below with your GitOps repo name)
                        git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/$GIT_USERNAME/GitOps-argo-cd.git HEAD:master
                    """
                }
            }
        }
    }
}
