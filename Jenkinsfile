pipeline {
    agent any

    stages {

        stage('Checkout'){
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/felipefrmelo/multi-docker']]])
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