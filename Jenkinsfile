pipeline {
    agent any

    tools {
        // These MUST match the names in 'Manage Jenkins > Tools'
        maven 'local_maven'
        jdk 'java21'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // 'clean package' creates the WAR file in the /target folder
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // 1. Stop Tomcat
                    sh '/opt/tomcat/bin/shutdown.sh || true'
                    
                    // 2. Delete the old deployment to ensure a clean slate
                    // 'my-app' matches the <finalName> from your pom.xml
                    sh 'rm -rf /opt/tomcat/webapps/my-app'
                    sh 'rm -f /opt/tomcat/webapps/my-app.war'
                    
                    // 3. Copy the new WAR file from the build folder to Tomcat
                    sh 'cp target/my-app.war /opt/tomcat/webapps/'
                    
                    // 4. Start Tomcat
                    sh '/opt/tomcat/bin/startup.sh'
                }
            }
        }
    }
}
