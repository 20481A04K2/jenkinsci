node {
    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh '''
                        git config user.email "vij1542044@gmail.com"
                        git config user.name "vamsi"

                        echo "===== Before Update ====="
                        cat deployment.yaml

                        # Update Docker image tag in deployment file
                        sed -i "s+sandip525/flaskapp.*+sandip525/flaskapp:${DOCKERTAG}+g" deployment.yaml

                        echo "===== After Update ====="
                        cat deployment.yaml

                        git add deployment.yaml
                        git commit -m "Updated image tag via Jenkins build: ${BUILD_NUMBER}"
                        git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/$GIT_USERNAME/jenkinsci.git HEAD:main
                    '''
                }
            }
        }
    }
}
