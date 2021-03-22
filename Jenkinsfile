pipeline {
    agent any 

    stages {
        stage('Checkout Source') {
            steps {
                gii url: 'https://github.com/ademarbarreto/pedelogo-catalogo.git', branch: 'master'
            }
        }
       stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("ademarbarreto/api-produto:${env.BUILD_ID}",
                        '-f ./scr/PedeLogo.Catalogo.Api/Dockerfile .')
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
    }
}
