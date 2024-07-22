pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
		// Run the test script located at './jenkins/scripts/test.sh'
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}