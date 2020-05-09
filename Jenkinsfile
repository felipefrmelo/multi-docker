pipeline {
    agent none 
    checkout([ $class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/felipefrmelo/multi-docker/settings/hooks/209556755']]])
    stages {
        stage('Example Build') {
            def dockerfile = 'Dockerfile.dev'
            def testImage = docker.build("test-image", "-f ${dockerfile} ./client") 
            testImage.inside {
                sh 'npm run test'
            }
          
        }
        stage('Example Test') {
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
            }
        }
    }
}

