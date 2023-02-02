
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
                 sh 'mvn clean install'
           }
       } 
    }
}  
