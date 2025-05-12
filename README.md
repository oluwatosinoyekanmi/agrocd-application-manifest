<!-- agrocd-application-manifest -->
# Image updater stage
```
 environment {
    GIT_REPO_NAME = "agrocd-application-manifest"
    GIT_USER_NAME = "oluwatosinoyekanmi"
  }
    stage('Checkout Code') {
      steps {
        git branch: 'main', url: 'https://github.com/oluwatosinoyekanmi/oluwatosinoyekanmi'
      }
    }

    stage('Update Deployment File') {
      steps {
        script {
          withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
            // Determine the image name dynamically based on your versioning strategy
            NEW_IMAGE_NAME = "ziemieu86/tetris77:latest"

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

```
