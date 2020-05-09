node {
    checkout scm

    stages {
        stage('test') {

            def dockerfile = 'Dockerfile.dev'
            def testImage = docker.build("test-image", "-f ${dockerfile} ./client") 

            testImage.inside {
                sh 'npm run test'
            }
        }

        // stage('publish images') {

        // }

    }
}