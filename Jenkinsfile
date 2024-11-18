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
        stage('Setup Git') {
            steps {
                sh 'git config --global --add safe.directory /var/jenkins_home/caches/git-a661cc6bfc9e076b12fd1311d4b932a0'
            }
        }
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                            -o './target'
                            -s './'
                            -f 'ALL' 
                            --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                
                // Ensure that the report is found in the target folder
                dependencyCheckPublisher pattern: '**/target/dependency-check-report.xml'
            }
        }
    }
}