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
                    dockerapp = docker.build("ademarbarretop/api-produto:versao-teste",
                    '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
                }
            }
        }
       stage('Push Image') {
            steps {
                script {
                        docker.withRegistry('http://registry.hub.docker.com', 'dockerhub') {    
                        dockerapp.push('latest')
                        dockerapp.push('versao-teste')  
                     }   
                }
            }
        }                        
    }
}
