pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/your-image'
        DOCKER_CREDENTIALS_ID = 'your-dockerhub-credentials-id'
        KUBECONFIG_CREDENTIALS_ID = 'your-kubeconfig-credentials-id'
        EKS_CLUSTER_NAME = 'your-eks-cluster-name'
    }

    stages {
        stage('Build Docker image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}:${env.BUILD_ID}").push()
                    }
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIALS_ID}", variable: 'KUBECONFIG')]) {
                    sh "kubectl config use-context ${EKS_CLUSTER_NAME}"
                    sh "kubectl set image deployment/your-deployment your-container=${DOCKER_IMAGE}:${env.BUILD_ID} --record"
                }
            }
        }
    }
}