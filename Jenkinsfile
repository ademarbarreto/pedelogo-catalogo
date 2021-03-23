pipeline {
    agent any 

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/ademarbarreto/pedelogo-catalogo.git', branch: 'master'
            }
        }
       stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("ademarbarretp/api-produto:${env.BUILD_ID}",
                    '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                        docker.withRegistry('http://registry.hub.docker.com', 'dockerhub') {   
                        dockerapp.push('latest')     
                        dockerapp.push("${env.BUILD_ID}")  
                    }   
                }
            }
        }      

        stage('Deploy kubernetes') {
            agent {
                kubernetes {
                    cloud 'kubernetes'
                }
            }
            environment {
                tag_version="${env.BUILD_ID}"
            }
            steps {
                script {
                    sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/api/deployment.yaml'
                    sh 'cat ./k8s/api/deployment.yaml'
                    kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig')
                }
            }
        }                    
    }
}    