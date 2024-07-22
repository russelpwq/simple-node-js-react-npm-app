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
		// Run 
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}