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
                script{
                    def dockerfile = 'Dockerfile.dev'
                    def testImage = docker.build("felipefrmelo/react-test", "-f ./client/${dockerfile} ./client") 
                }
                sh "docker run -e CI=true felipefrmelo/react-test npm test"
            }
        }

        // stage('Test') {
        //     steps{
        //         script{

        //         def dockerfile = 'Dockerfile.dev'
        //         def testImage = docker.build("test-image", "-f ${dockerfile} ./client") 

        //         testImage.inside {
        //             sh 'npm run test'
        //         }
        //         }
        //     }
        // }
        
    }
}