pipeline {
  agent any
  tools {
    maven 'maven3.8.7'
  }
//     environment {
// //        AWS_ACCOUNT_ID= "803561623563"
// //        AWS_DEFAULT_REGION="ap-south-1"
// //        IMAGE_REPO_NAME= "ecrpipeline"
// //        IMAGE_TAG= "latest"
// //        REPOSITORY_URI= "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
//     }
    
    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/shrasyntex00/java-web-app.git'
            } 
        }
        
        // stage('Unit Testing') {
        //     steps {
        //       sh 'mvn test'
        //     }
        // }
        
        // stage('Integration Testing'){
        //     steps {
        //         sh 'mvn verify -DskipUnitTests'
        //     }
        // }
      stage('SonarQube Analysis'){
            steps {
                    withSonarQubeEnv(installationName: 'demoapp') {
                      sh 'mvn sonar:sonar'
                    }  
                
            }
        }
        
        stage('Quality Gate Analysis'){
            steps{
                    waitForQualityGate abortPipeline: true 
            }
        }
        
        stage('Build') {
            steps {
              sh 'mvn clean package'
            }
        }
      
      stage('deploy to tomcat') {
            steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcatcred', path: '', url: 'http://15.168.8.30:8080/')], contextPath: null, war: '**/*.war'
            }
        }

//         stage('Upload war to Nexus'){
//             steps {
//                 script{                                       
//                     def readPomVersion = readMavenPom file: 'pom.xml'
// //                     def nexusRepo = readPomVersion.version.endsWith("RELEASE") ? "skan-snapshot" : "skanjob"
//                     nexusArtifactUploader artifacts: [
//                           [
//                             artifactId: 'java-web-app',
//                             classifier: '',
//                             file: "target/java-web-app-${readPomVersion.version}.war", 
//                             type: 'war'
//                           ]
//                         ], 

//                         credentialsId: 'nexuscredentials', 
//                         groupId: 'com.mt',
//                         nexusUrl: '13.208.173.119:8081', 
//                         nexusVersion: 'nexus3',
//                         protocol: 'http',
//                         repository: 'test-release', 
//                         version: "${readPomVersion.version}"
//                 }
//             }
//         }
      
      
       
//       stage('deploy with ansible'){
//             steps {
//             ansiblePlaybook credentialsId: 'ansiblecredential', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'hosts', playbook: 'appdeploy.yaml'
//             }      
//       }
        
        
        
        // stage('push nexus artifact'){
        //     steps {
        //         sh 'mvn clean deploy'
        //     }
        // }
        
        
        
        // stage('build docker image') {
        //     steps {
        //         script{
        //             dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        //         }
        //     }
        // }

        // stage('DockerHUB LOGIN & push image') {
        //     steps {
        //         withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'dockerhubcredentials')]) {
        //             sh "docker login -u account1996 -p ${dockerhubcredentials}"  
        //         }
        //             sh 'docker push account1996/java:1'
      	//   }
        // }
        
        // stage('Loggingto AWS ECR') {
        //     steps {
        //         sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 803561623563.dkr.ecr.ap-south-1.amazonaws.com"
        //     }
        // }
        
        // stage('Pushingto ECR') {
        //   steps{
        //        sh "docker build -t ecrpipeline ."
        //        sh "docker tag ecrpipeline:latest 803561623563.dkr.ecr.ap-south-1.amazonaws.com/ecrpipeline:latest"
        //        sh "docker push 803561623563.dkr.ecr.ap-south-1.amazonaws.com/ecrpipeline:latest"
        //     }
        // }
        
        // stage('K8S Deploy'){
        //     steps{
        //         // kubernetesDeploy(
        //         //   config: 'java-web-app-docker/javawebapp-deployment.yml',
        //         //   kubeconfigId:'K8S',
        //         //   enableConfigSubstitution: true
        //         // )
                
        //       sh 'kubectl apply -f javawebapp-deployment.yml'
        //     }
        // }
    }
}
 
