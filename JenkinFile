pipeline {
    agent any
  
    environment {
        DOCKER_IMAGE = "ephrash1/product3_api"
        DOCKER_TAG = "latest"
        EKS_CLUSTER_NAME = "ephrash-cluster"
        KUBECONFIG = credentials('k8s-cred')
    }
    tools{
        maven "Maven"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Muthaiyyan/01_products_api_Vinod.git'
            }
        }
        stage('Maven Build'){
            steps{
             sh 'mvn clean package'
            }
        }
        stage('Docker Image'){
            steps{
             sh 'docker build -t ephrash1/product3_api .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
        stage('Deploy to Kubernetes (Using Local Deployment YAML)') {
            steps {
                sh "kubectl apply -f /var/lib/jenkins/workspace/Project3BE/Deployment.yaml"
            }
        }
    }
 }
