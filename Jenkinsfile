pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Run the test script
                sh './jenkins/scripts/test.sh'
            }
        }

        stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }     

        stage('Deliver') { 
            steps {
                // Run the deliver script
                sh './jenkins/scripts/deliver.sh'
                
                // Wait for user input before proceeding
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                
                // Run the kill script
                sh './jenkins/scripts/kill.sh'
            }
        }
    }

    post {
        success {
            // Publish the DependencyCheck report if the build is successful
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }
    }
}
