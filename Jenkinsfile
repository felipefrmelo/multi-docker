pipeline {
    agent any

    stages {

        stage('Checkout'){
            checkout scm
           
        }

        stage('Test') {
            def dockerfile = 'Dockerfile.dev'
            def testImage = docker.build("test-image", "-f ${dockerfile} ./client") 

            testImage.inside {
                sh 'npm run test'
            }
        }
        
    }
}