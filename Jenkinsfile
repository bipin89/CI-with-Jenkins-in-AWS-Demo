def customImage 
pipeline {
    agent any
      stages {
        stage('SCM Pull') { 
            steps {
                dir('artifacts'){
                    git url: 'https://github.com/edureka-git/DevOpsClassCodes.git'
                }
                
            }
        }
        stage('Compile') { 
            steps {
             echo "Static code analysis"  
             dir('artifacts'){
                withMaven(maven: 'mymaven') {
                  sh 'mvn compile' 
               
             } 
             
            }
        }
    }
        stage('Test') { 
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
    }

/*        stage('Build and Sonarcube Analysis') { 
            steps {
             echo "Static code analysis"  
             dir('artifacts'){
                withMaven(maven: 'mymaven') {
                 // sh 'mvn sonar:sonar -Dsonar.projectKey=devops-casestudy -Dsonar.host.url=http://35.200.254.182:9000 -Dsonar.login=571a21bbd37e72fe471a9dd4f5953b9a226b6744'                 }
             } 
             
            }
        }
    }
    stage('Compile & Package') { 
            steps {
             echo "Static code analysis"  
             dir('artifacts'){
                withMaven(maven: 'mymaven') {
                 // sh 'mvn compile' 
                  sh 'mvn package'  
               
             } 
             
            }
        }
    }
    stage('Build image') { 
        //agent { label 'docker' }
      steps {
             echo "Build the docker file"  
             script{
                
                 sh 'cp ${JENKINS_HOME}/workspace/${JOB_NAME}/artifacts/target/addressbook.war .'
                 customImage = docker.build("bipin89/addressbook:${BUILD_NUMBER}")
                 echo customImage
                
             }
        }
    }
    stage('Deploy Image') { 
        //agent { label 'docker' }
      steps {
             echo "Build the docker file"  
             script{
                 
                 docker.withRegistry( '', 'DOCKERHUBLOGIN' ) {
                           customImage.push()
                }
             }
        }
    }

}
    post {  
          
         success {  
             echo 'This will run only if successful'  
             mail bcc: '', body: " Build Result : Success <br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8',  mimeType: 'text/html', replyTo: '', subject: "Success CI: Project name -> ${env.JOB_NAME}", to: "bipin89@gmail.com";  
         }  
         failure {  
            echo " Sending mail with failure cause"
            mail bcc: '', body: "Build Result : Failure<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: 'bipinrajan89@gmail.com', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "bipin89@gmail.com";  

         }  
     }   
}
*/
