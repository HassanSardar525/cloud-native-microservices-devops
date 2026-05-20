pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'hassansardar525'
        APP_VERSION = '1.0.0'
        APP_NAME = 'shopcloud'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '--- Checking out source code ---'
                checkout scm
                sh 'echo "Branch: ${GIT_BRANCH}"'
                sh 'echo "Commit: ${GIT_COMMIT}"'
            }
        }

        stage('Lint & Test') {
            steps {
                echo '--- Running HTML lint checks ---'
                sh '''
                    PASS=true
                    for file in $(find src -name "*.html"); do
                        if grep -q "<!DOCTYPE html>" "$file"; then
                            echo "✔ $file passed lint"
                        else
                            echo "✘ $file failed lint"
                            PASS=false
                        fi
                    done
                    if [ "$PASS" = false ]; then
                        exit 1
                    fi
                '''

                echo '--- Verifying CSS files exist ---'
                sh '''
                    for service in frontend user-service product-service order-service notification-service; do
                        if [ -f "src/$service/style.css" ]; then
                            echo "✔ src/$service/style.css found"
                        else
                            echo "✘ src/$service/style.css MISSING"
                            exit 1
                        fi
                    done
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo '--- Building Docker images ---'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/frontend:${APP_VERSION} ./src/frontend'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/user-service:${APP_VERSION} ./src/user-service'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/product-service:${APP_VERSION} ./src/product-service'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/order-service:${APP_VERSION} ./src/order-service'
                sh 'docker build -t ${DOCKERHUB_USERNAME}/notification-service:${APP_VERSION} ./src/notification-service'
                echo '--- All images built successfully ---'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '--- Pushing images to Docker Hub ---'
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push ${DOCKERHUB_USERNAME}/frontend:${APP_VERSION}'
                    sh 'docker push ${DOCKERHUB_USERNAME}/user-service:${APP_VERSION}'
                    sh 'docker push ${DOCKERHUB_USERNAME}/product-service:${APP_VERSION}'
                    sh 'docker push ${DOCKERHUB_USERNAME}/order-service:${APP_VERSION}'
                    sh 'docker push ${DOCKERHUB_USERNAME}/notification-service:${APP_VERSION}'
                    sh 'docker logout'
                }
                echo '--- All images pushed successfully ---'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo '--- Deploying to Kubernetes ---'
                sh 'kubectl apply -f k8s/configmap.yaml'
                sh 'kubectl apply -f k8s/frontend-deployment.yaml'
                sh 'kubectl apply -f k8s/user-deployment.yaml'
                sh 'kubectl apply -f k8s/product-deployment.yaml'
                sh 'kubectl apply -f k8s/order-deployment.yaml'
                sh 'kubectl apply -f k8s/notification-deployment.yaml'
                echo '--- Waiting for rollout to complete ---'
                sh 'kubectl rollout status deployment/frontend'
                sh 'kubectl rollout status deployment/user-service'
                sh 'kubectl rollout status deployment/product-service'
                sh 'kubectl rollout status deployment/order-service'
                sh 'kubectl rollout status deployment/notification-service'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo '--- Verifying all pods are running ---'
                sh 'kubectl get pods'
                sh 'kubectl get services'
                echo '--- Deployment verified successfully ---'
            }
        }

        stage('Notify') {
            steps {
                echo '--- Sending deployment notification ---'
                echo "✔ ShopCloud v${APP_VERSION} deployed successfully"
                echo "Branch: ${GIT_BRANCH}"
                echo "Commit: ${GIT_COMMIT}"
                echo "Build: ${BUILD_NUMBER}"
            }
        }
    }

    post {
        success {
            echo '✔ Pipeline completed successfully — all stages passed'
        }

        failure {
            echo '✘ Pipeline failed — initiating rollback'
            sh '''
                kubectl rollout undo deployment/frontend || true
                kubectl rollout undo deployment/user-service || true
                kubectl rollout undo deployment/product-service || true
                kubectl rollout undo deployment/order-service || true
                kubectl rollout undo deployment/notification-service || true
                echo "Rollback completed"
            '''
        }

        always {
            echo '--- Cleaning up ---'
            sh 'docker system prune -f || true'
            echo "Build ${BUILD_NUMBER} finished at ${new Date()}"
        }
    }
}