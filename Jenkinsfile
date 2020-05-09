pipeline {
    agent any

    environment {
        DOCKER = credentials('DockerHub')
    }

    stages {

        stage('Checkout'){
            steps {
                checkout scm
            }
           
        }

        stage('Test') {
            steps{
                script{
                    def dockerfile = 'Dockerfile.dev'
                    def testImage = docker.build("felipefrmelo/react-test", "-f ./client/${dockerfile} ./client") 
                }
                sh "docker run -e CI=true felipefrmelo/react-test npm test"
            }
        }

        stage('build and publish') {
            steps{
                script{
                def  felipefrmelo = "663375400923.dkr.ecr.us-east-1.amazonaws.com"
                
                def client = docker.build("${felipefrmelo}/multi-client", "./client")
                def nginx = docker.build("${felipefrmelo}/multi-nginx", "./nginx")
                def server = docker.build("${felipefrmelo}/multi-server", "./server")
                def worker = docker.build("${felipefrmelo}/multi-worker", "./worker") 

                 docker.withRegistry("https://${felipefrmelo}/multi-docker", 'ecr:us-east-1:AWS_CRED'){
                        nginx.push()
                        server.push()
                        worker.push()
                        client.push()
                    }
                }
            }
        }
        
    }
}