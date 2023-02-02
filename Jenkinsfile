
pipeline {
    agent any
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
                    -Dsonar.host.url=http://13.232.188.97:9000 \
                        
                }
           }
       } 
    }
}  
