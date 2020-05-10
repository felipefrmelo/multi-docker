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
                
                def client = docker.build("multi-client", "./client")
                def nginx = docker.build("multi-nginx", "./nginx")
                def server = docker.build("multi-server", "./server")
                def worker = docker.build("multi-worker", "./worker") 

                 docker.withRegistry("https://${felipefrmelo}", 'ecr:us-east-1:AWS_CRED'){
                        nginx.push()
                        server.push()
                        worker.push()
                        client.push()
                    }
                }
            }
        }

        stage('deploy') {
            steps{
               
                step([$class: 'AWSEBDeploymentBuilder', bucketName: 'elasticbeanstalk-us-east-1-663375400923', credentialId: 'ecr:us-east-1:AWS_CRED', environmentName: 'MultiDocker-env', versionLabelFormat: '${GIT_COMMIT}', applicationName: 'multi-docker'])
            }
        }
        
    }
}
