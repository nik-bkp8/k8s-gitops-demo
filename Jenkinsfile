pipeline {
    agent any

    stages {
        stage('Select Environment') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo "Deploying to DEV environment (k8s/dev)"
                    } else if (env.BRANCH_NAME == 'prod') {
                        echo "Deploying to PROD environment (k8s/prod)"
                    } else {
                        echo "Not a deployable branch. Skipping deployment."
                    }
                }
            }
        }
    }
}
