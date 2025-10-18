pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from SCM...'
                // The 'scm' step uses the SCM configuration defined in the job
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Compiling and running unit tests...'
                // Execute the standard Maven command
                sh 'mvn clean install -DskipTests' 
            }
        }
        stage('Run Application') {
            steps {
                echo 'Packaging the final application (JAR/WAR)...'
                // Run full tests, then package the application
                sh 'mvn package'
            }
        } 
	}
    post {
	
		// Always execute this block
        always {
            echo 'Pipeline finished. Cleaning up workspace...'
        }
        
        // Execute only if the entire pipeline was successful
        success {
            echo 'Build Successful! Archiving artifact.'
            // Archive the built artifact (e.g., the JAR file)
            // Adjust the path pattern if you build a WAR or other file
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }

        // Execute only if the pipeline failed in any stage
        failure {
            echo 'Pipeline FAILED! Review the console output.'
        }
		
		always {
			junit 'target/surefire-reports/*.xml'
		}
		
	}
}