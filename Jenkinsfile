pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'demoapp'
        DOCKER_CREDENTIALS_ID = 'your-dockerhub-credentials-id'
        // KUBECONFIG_CREDENTIALS_ID = 'your-kubeconfig-credentials-id'
        KUBECONFIG_CREDENTIALS = credentials('KUBECONFIG_CREDENTIALS_ID')
        EKS_CLUSTER_NAME = 'my-cluster'
        region = 'us-east-1'
        accountID = '4997-5607-6901'
    }

    stages {
        // stage('Build Docker image') {
        //     steps {
        //         script {
        //             docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
        //         }
        //     }
        // }

        // stage('Push Docker image') {
        //     steps {
        //         script {
        //             sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 499756076901.dkr.ecr.us-east-1.amazonaws.com"
        //             sh "docker tag demoapp:${env.BUILD_ID} 499756076901.dkr.ecr.us-east-1.amazonaws.com/demoapp:latest"
        //             sh "docker push 499756076901.dkr.ecr.us-east-1.amazonaws.com/demoapp:latest"
        //             // Authenticate to ECR
        //             //sh "aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${accountID}.dkr.ecr.${region}.amazonaws.com"

        //             // Tag the image with the ECR repository URL
        //             //sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_ID} ${accountID}.dkr.ecr.${region}.amazonaws.com/demoapp:${env.BUILD_ID}"

        //             // Push the image
        //              //sh "docker push ${accountID}.dkr.ecr.${region}.amazonaws.com/demoapp:${env.BUILD_ID}"
        //         }
        //     }
        // }

        stage('Deploy to EKS') {
            steps {
                withCredentials([file(credentialsId: "${KUBECONFIG_CREDENTIALS_ID}", variable: 'KUBECONFIG_CREDENTIALS')]) {
                    sh "kubectl --kubeconfig=$KUBECONFIG_CREDENTIALS get pods"
                    sh "kubectl apply -f k8s-manifest/namespace.yaml"
                }
            }
        }

        // stage('Deploy to EKS') {
        //     steps {
        //             script{
        //                 sh "aws eks --region us-east-1 update-kubeconfig --name my-cluster"
        //                 sh "hostname"
        //                 sh "kubectl get pods"
        //                 sh "kubectl config use-context my-cluster"
        //                 sh "kubectl apply -f k8s-manifest/namespace.yaml"
        //                 sh "kubectl apply -f k8s-manifest/deployment.yaml -n github-copilot "
        //                 sh "kubectl apply -f k8s-manifest/service.yaml -n github-copilot"
        //                 sh "kubectl apply -f k8s-manifest/hpa.yaml -n github-copilot"
        //                 sh "kubectl apply -f k8s-manifest/ingress.yaml -n github-copilot"
        //             }
        //         }
        //     }
        }
    }
