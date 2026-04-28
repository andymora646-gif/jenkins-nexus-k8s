pipeline {
    agent any
    tools { maven 'maven' }
    environment {
        NEXUS_URL = "nexus-service.nexus:8081" 
        NEXUS_CREDENTIAL_ID = "nexus-credentials"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build JAR') {
            steps { sh 'mvn clean package -DskipTests' }
        }
        stage('Push to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: "nexus3",
                    protocol: "http",
                    nexusUrl: "${NEXUS_URL}",
                    groupId: "com.example",
                    version: "1.0-${BUILD_NUMBER}", // Use Build Number to avoid "Redeploy" errors
                    repository: "maven-releases",
                    credentialsId: "${NEXUS_CREDENTIAL_ID}",
                    artifacts: [[artifactId: "my-app", file: "target/my-app-1.0.jar", type: "jar"]]
                )
            }
        }
    }
}
