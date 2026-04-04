pipeline{
    agent any
    environment{
        IMAGE_NAME = "mubha/terraform"
        TAG = "latest"
        
        DOCKER_IMAGE = "mubha/terraform_jenkinserver:latest"
        KUBECONFIG = "/home/ec2-user/.kube/config"

        EKS_CLUSTER = 'your-cluster-name'
        AWS_REGION  = 'your-region'
    }
    stages{
        stage('git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/devopsmuthuraman/Trendapp.git'
            }
        }
        stage('Build image'){
            steps{
                script{
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'aa043f6e-7ddd-44ad-b1d2-5ffe5ab7f1a1',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        docker login -u $DOCKER_USER -p $DOCKER_PASS
                        docker push mubha/terraform
                    '''
                    }
            }
        }
        stage('Deploy to EKS') {
            steps {
                script {
                    sh """
                        aws eks update-kubeconfig --name ${EKS_CLUSTER} --region ${AWS_REGION}
                        kubectl get nodes
                        kubectl apply -f deployment.yml
                    """
                }
            }
        }
    }
}