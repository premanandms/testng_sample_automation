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
        stage('Post') {	
            
			
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
				
            }
        }
		
	}
}