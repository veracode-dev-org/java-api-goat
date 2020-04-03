pipeline {
    agent {
        docker {
            image 'maven:3-jdk-8' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                wrap([$class: 'VeracodeInteractiveBuildWrapper', location: 'host.docker.internal', port: '10010']) {
                    sh 'curl -sSL https://s3.us-east-2.amazonaws.com/app.veracode-iast.io/iast-ci.sh | sh'
                    sh 'mvn -DIASTAGENT_LOGGING_STDERR_LEVEL=info --debug --log-file ${BUILD_TAG}.mvn.log test'
                }
            }
        }
        stage('Deploy') { 
            steps {
                sh 'echo mvn install would run here...'
            }
        }
    }
}