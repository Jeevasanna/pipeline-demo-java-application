
pipeline {
    agent any
//     tools {              //Configuring this Pipeline to use the Maven version matching maven 3.8.7 & configured same in "Global tool configuration"
//     maven 'Maven 3.8.7'
//     }
    stages {
      stage('Git checkout') {    //Getting the source code from my github repo 'main' branch
           steps {
               git branch: 'main', url: 'https://github.com/Jeevasanna/pipeline-demo-java-application.git'
           }
       }
       stage('Build artifact') {     //This will compile and generate a war file as a package for my java application
            steps {
                 sh 'mvn clean package'
           }
       }
       stage('sonar Analysis') {    //Code Quality Assurance tool that collects and analyzes source code, and provides reports for the code quality of your project
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
       stage('Quality Gate Analysis'){    //its for quality policy in our org & tells whether my project is ready for release
            steps{
                    waitForQualityGate abortPipeline: true 
           }
       } 
       stage('Upload war to Nexus'){     //This will push the already generated war file to a prescribed folder in Nexus repository manager 
            steps{
                nexusArtifactUploader artifacts: [    //using this plugin, am uploading my artifact to nexus , got this info from snippet generator
                    [
                        artifactId: 'java-web-app',   //got from pom.xml
                        classifier: '', 
                        file: 'target/java-web-app-1.0.0.war',   //jenkins workspace war file generated location
                        type: 'war'
                    
                    ]
                ], 
                credentialsId: 'nexus3',     //nexus id , where my username & pwd defined
                groupId: 'com.mt',                 // got from pom.xml
                nexusUrl: '43.204.98.82:8081',     //my nexusurl with public-ip, bcoz my nexus and jenkins are not in same network
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'pipeline-demo-java-application-release',    //my repo name in nexus
                version: '1.0.0'
            }    
        }
    }
}  
