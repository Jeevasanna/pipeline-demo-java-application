
pipeline {
    agent any
    tools {
        maven 'maven 3.6.3'
    }
    stages {
      stage('Git checkout') {
           steps {
               git branch: 'main', url: 'https://github.com/Jeevasanna/pipeline-demo-java-application.git'
           }
       }
       stage('Build artifact') {
            steps {
                 sh 'mvn clean package'
           }
       }
       stage('sonar Analysis') {
            steps{
                withSonarQubeEnv('sonarqube') {
                   sh 'mvn clean verify sonar:sonar \
                    -Dsonar.projectName=pipeline-demo-java-application-1 \
                    -Dsonar.projectKey=java-project \
                    -Dsonar.login=squ_4753f857e058e9894a7e54e405251b1672a20d6f \
                    -Dsonar.host.url=http://13.232.188.97:9000'
                        
                }
           }
       }
       stage('Upload war to Nexus'){
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'java-web-app', 
                        classifier: '', 
                        file: 'target/java-web-app-1.0.0.war', 
                        type: 'war'
                    
                    ]
                ], 
                credentialsId: 'nexus3', 
                groupId: 'com.mt', 
                nexusUrl: '43.204.98.82:80:8081', 
                nexusVersion: 'nexus2', 
                protocol: 'http', 
                repository: 'http://43.204.98.82:8081/repository/pipeline-demo-java-application-release/', 
                version: '1.0.0'
            }    
        } 
    }
}  
