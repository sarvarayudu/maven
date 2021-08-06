pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/sarvarayudu/maven.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
                
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp sarvarayudu/javaapp:latest'
                //sh 'docker tag samplewebapp nikhilnidhi/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
       // withDockerRegistry([ credentialsId: "DOCKER_HUB", url: "https://registry.hub.docker.com" ]) {
         // sh  'docker push ishaqmd/jenkins:latest'
          //sh  'docker push ishaqmd/jenkins:$BUILD_NUMBER'
	withCredentials([string(credentialsId: 'Docker_User', variable: 'Docker_User'), string(credentialsId: 'Password', variable: 'Docker_Password')]) {
            sh 'docker login -u $Docker_User -p $Docker_Password'
              sh  'docker push sarvarayudu/javaapp:latest'
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8008:8080 sarvarayudu/javaapp"
				sh 'sleep 10'
				sh 'curl localhost:8080/hello'
				sh 'sleep 10'
				
 
            }
        }
	 stage('Test the container') {
           steps {
                
               sh 'curl localhost:8080/hello'
				sh 'sleep 10'
               
          }
        }
	 stage('Delete the container') {
           steps {
                
                sh 'docker stop javaapp'
				sh 'docker rm javaapp'
               
          }
        }
 //stage('Run Docker container on remote hosts') {
             
   //         steps {
     //           sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 ishaqmd/jenkins"
 
           // }
        //}
    }
	}
