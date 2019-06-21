def dockerImage 
pipeline {
    environment {
    registry = "bipin89/addressbook"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
    agent any
      stages {
        stage('SCM Pull') { 
            steps {
                dir('artifacts'){
                    git url: 'https://github.com/edureka-git/DevOpsClassCodes.git'
                }
                
            }
        }
        stage('Project Compile') { 
            steps {  
             dir('artifacts'){
                withMaven(maven: 'mymaven') {
                  sh 'mvn compile' 
               
             } 
             
            }
        }
    }
        stage('Unit Testing') { 
            steps {
             echo "Testing"  
             dir('artifacts'){
                withMaven(maven: 'mymaven') 
                {
                  sh 'mvn test'
                } 
             
              }
            }
             post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'

                }
           }
       }
    

        stage('Sonarcube Analysis') { 
            steps {
             echo "Static code analysis"  
             dir('artifacts'){
                withMaven(maven: 'mymaven') {
                 sh 'mvn sonar:sonar -Dsonar.projectKey=CI-with-Jenkins-in-AWS-Demo -Dsonar.host.url=http://34.93.62.28:9000 -Dsonar.login=7f07d49b0c96adcc7a75a354aea2b5dd25e9ed44'   
             } 
             
            }
        }
    } 
    stage('Package') { 
            steps {
             echo "Generating WAR file"  
             dir('artifacts'){
                withMaven(maven: 'mymaven') {
                  
                  sh 'mvn package'  
               
             } 
             
            }
        }
    }
  
    stage('Custom Image Build') { 
        //agent { label 'docker' }
      steps {
             echo "Build the docker file"  
             script{
                
                 sh 'cp ${JENKINS_HOME}/workspace/${JOB_NAME}/artifacts/target/addressbook.war .'
                 dockerImage = docker.build registry + ":$BUILD_NUMBER"
                
                 //echo dockerImage
                
             }
        }
    }
    stage('Deploy Image') { 
        //agent { label 'docker' }
      steps {
             echo "Pushing to DockerHub"  
             script{
                 docker.withRegistry( '', registryCredential ) {
                 dockerImage.push()
                 }
                 
             }
        }
    }

}
    post {  
          
       success {
        mail from: "bipinrajan89@gmail.com"
	     to: "bipin.rajan@delta.com",
             subject: "Pipeline Success: ${currentBuild.fullDisplayName}",
             body: "Build Result : Success<br> Jobname: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL of the build: ${env.BUILD_URL}"
    }
 
         failure {  
            echo "Failure Notification"
            mail body: "Build Result : Failure<br> Jobname: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL of the build: ${env.BUILD_URL}", charset: 'UTF-8', from: "bipinrajan89@gmail.com", mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "bipin.rajan@delta.com";  

         }  
     }   
}
