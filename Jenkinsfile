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

                def client = docker.build("multi-client", "./client")
                def nginx = docker.build("multi-nginx", "./nginx")
                def server = docker.build("multi-server", "./server")
                def worker = docker.build("multi-worker", "./worker") 

                    withDockerRegistry(credentialsId: 'DockerHub', url: 'registry.hub.docker.com/') {
                        client.push()
                    }
                }
            }
        }
        
    }
}