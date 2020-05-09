pipeline {
    agent any

    stages {

        stage('Checkout'){
            steps {
                checkout scm
            }
           
        }

        stage('Test') {
            steps{
                def dockerfile = 'Dockerfile.dev'
                def testImage = docker.build("test-image", "-f ${dockerfile} ./client") 

                testImage.inside {
                    sh 'npm run test'
                }
            }
        }
        
    }
}