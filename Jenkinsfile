pipeline {
    agent any

    environment {
        KCFG = 'C:\\Windows\\System32\\config\\systemprofile\\.kube\\config'
    }

    stages {
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'dev') {
                        echo 'Deploying to DEV environment...'
                        sh """
                            echo 'Using kubeconfig: ${KCFG}'
                            kubectl --kubeconfig="${KCFG}" config use-context docker-desktop

                            echo 'Applying namespace...'
                            kubectl --kubeconfig="${KCFG}" apply -f k8s/dev/namespace.yaml

                            echo 'Waiting for namespace to be ready...'
                            sleep 3

                            echo 'Applying all dev manifests...'
                            kubectl --kubeconfig="${KCFG}" apply -f k8s/dev/

                            echo 'Waiting for rollout...'
                            kubectl --kubeconfig="${KCFG}" rollout status deployment/polling-app -n polling-app-dev || true
                        """
                    } else if (env.BRANCH_NAME == 'prod') {
                        echo 'Deploying to PROD environment...'
                        sh """
                            echo 'Using kubeconfig: ${KCFG}'
                            kubectl --kubeconfig="${KCFG}" config use-context docker-desktop

                            echo 'Applying namespace...'
                            kubectl --kubeconfig="${KCFG}" apply -f k8s/prod/namespace.yaml

                            echo 'Waiting for namespace to be ready...'
                            sleep 3

                            echo 'Applying all prod manifests...'
                            kubectl --kubeconfig="${KCFG}" apply -f k8s/prod/

                            echo 'Waiting for rollout...'
                            kubectl --kubeconfig="${KCFG}" rollout status deployment/polling-app -n polling-app-prod || true
                        """
                    } else {
                        echo "Branch ${env.BRANCH_NAME} is not deployable. Skipping."
                    }
                }
            }
        }
    }
}
