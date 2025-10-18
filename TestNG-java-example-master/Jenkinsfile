pipeline {
    agent any // Specifies that the job can run on any available agent/node

    // Define environment variables, useful for tools or paths
    environment {
        // Assume you have configured Maven and JDK in 'Manage Jenkins' -> 'Global Tool Configuration'
        // Replace 'JDK_17' and 'Maven_3' with the names you used in the Global Tool Configuration
        JAVA_TOOL = 'JDK_17' 
        MAVEN_TOOL = 'Maven_3' 
    }

    stages {
        // 1. Checkout Stage: Get the code from the Git repository
        stage('Checkout') {
            steps {
                // Assuming the pipeline is configured to use SCM (Git)
                // This step pulls the latest code from the repository
                checkout scm
            }
        }

        // 2. Build Stage: Compile the Java code using Maven
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Use the Maven tool defined in the environment
                withMaven(jdk: env.JAVA_TOOL, maven: env.MAVEN_TOOL) {
                    // Goal: clean the target directory, then compile the project
                    sh 'mvn clean compile'
                }
            }
        }

        // 3. Test Stage: Run TestNG tests using Maven
        stage('Test') {
            steps {
                echo 'Running TestNG tests...'
                withMaven(jdk: env.JAVA_TOOL, maven: env.MAVEN_TOOL) {
                    // Goal: surefire:test runs the tests (TestNG is configured via Surefire plugin in pom.xml)
                    // The '-B' flag makes the output less verbose (batch mode)
                    sh 'mvn -B test' 
                }
            }
        }
    }

    // Post-build actions run after all stages, regardless of success/failure
    post {
        // Always run: Clean up workspace
        always {
            echo 'Cleaning up workspace...'
            // cleanWs() // Uncomment this line if you want to wipe the workspace completely after the job
        }

        // Run only if the pipeline succeeded
        success {
            echo 'Build and Tests succeeded! Generating reports.'
            // Archive the compiled artifact (e.g., WAR or JAR file)
            // Replace '**/target/*.jar' with your actual artifact path if needed
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            
            // Publish the TestNG test results for reporting
            // TestNG reports are usually in target/surefire-reports or target/failsafe-reports
            // Note: You need the JUnit Plugin (standardly installed) to process these XML reports
            junit '**/target/surefire-reports/*.xml' 
        }

        // Run only if the pipeline failed
        failure {
            echo 'Build or Test stage failed. Review console output for details.'
            // Optionally, send a notification (e.g., Slack, Email)
        }
    }
}