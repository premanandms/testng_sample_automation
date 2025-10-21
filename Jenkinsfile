pipeline {
    agent any

    tools {
        jdk 'JDK21'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from SCM...'
                // The 'scm' step uses the SCM configuration defined in the job
                git branch: 'main', url: 'https://github.com/premanandms/testng_sample_automation.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Compiling and running unit tests...'
                // Execute the standard Maven command
                bat 'mvn clean compile' 
            }
        }
        stage('Run Application') {
            steps {
                echo 'Packaging the final application (JAR/WAR)...'
                // Run full tests, then package the application
                bat 'mvn package'
            }
        } 
	stage('Test') {
            steps {
                echo 'Packaging the final application (JAR/WAR)...'
                // Run full tests, then package the application
                bat 'mvn teest'
            }
	    post {
		always {
		    junit 'target/surefire-reports/*.xml'
		}
	    }
        } 
   }
    post {		       
        // Execute only if the entire pipeline was successful
        success {
            echo 'Pipeline SUCCESS!'
        }
        // Execute only if the pipeline failed in any stage
        failure {
            echo 'Pipeline FAILED! Review the console output.'
        }		
    }
}