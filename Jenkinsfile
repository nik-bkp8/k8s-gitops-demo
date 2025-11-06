pipeline {
    agent any

    environment {
        KCFG = "C:\\Windows\\System32\\config\\systemprofile\\.kube\\config"
    }

    stages {
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo "Deploying to DEV environment..."
                        sh """
                            kubectl --kubeconfig="${KCFG}" config use-context docker-desktop
                            kubectl --kubeconfig="${KCFG}" apply -f k8s/dev/
                            kubectl --kubeconfig="${KCFG}" rollout status deployment/polling-app -n polling-app-dev || true
                        """
                    } else if (env.BRANCH_NAME == 'prod') {
                        echo "Deploying to PROD environment..."
                        sh """
                            kubectl --kubeconfig="${KCFG}" config use-context docker-desktop
                            kubectl --kubeconfig="${KCFG}" apply -f k8s/prod/
                            kubectl --kubeconfig="${KCFG}" rollout status deployment/polling-app -n polling-app-prod || true
                        """
                    } else {
                        echo "Not a deployable branch. Skipping deployment."
                    }
                }
            }
        }
    }
}
