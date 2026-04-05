pipeline{
    agent any
    environment{
        IMAGE_nAME = "terraform/nodeapp"
        TAG = "latest"
        
        DOCKER_IMAGE = "mubha/terraform_jenkinserver:latest"
        KUBECONFIG = "/home/ec2-user/.kube/config"
    }
    stages{
        stage('git clone'){
            steps{
                git branch: 'main', url: 'https://github.com/devopsmuthuraman/Test.git'
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
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                        docker push ${DOCKERHUB_REPO}:${TAG}
                    '''
                    }
            }
        }    
    }
}