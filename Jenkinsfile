pipeline {
    agent any

    stages {
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo "Deploying to DEV environment..."
                        sh '''
                            kubectl config use-context docker-desktop
                            kubectl apply -f k8s/dev/
                            kubectl rollout status deployment/polling-app -n polling-app-dev || true
                        '''
                    } else if (env.BRANCH_NAME == 'prod') {
                        echo "Deploying to PROD environment..."
                        sh '''
                            kubectl config use-context docker-desktop
                            kubectl apply -f k8s/prod/
                            kubectl rollout status deployment/polling-app -n polling-app-prod || true
                        '''
                    } else {
                        echo "Not a deployable branch. Skipping deployment."
                    }
                }
            }
        }
    }
}
