pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
    }
}