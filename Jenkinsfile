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
               	                 
          }
        }
     
       
      stage('Run Docker') {
             
            steps 
			{
                sh "docker run --name javaapp -d -p 8008:8080 sarvarayudu/javaapp"
				sh 'sleep 10'
				
				
 
            }
        }
	 
	 
	 stage('Test') {
           steps {
                
               sh 'curl localhost:8008/health'
				sh 'sleep 20'
               	                 
          }
        }
 
	 stage('Delete infra') {
           steps {
                
                sh 'docker stop javaapp'
				sh 'docker rm javaapp'
               	                 
          }
        }
	 
	 stage('Publish image to Docker Hub') {
          
            steps {
        
		withCredentials([string(credentialsId: 'Docker_User', variable: 'Docker_User'), string(credentialsId: 'Password', variable: 'Docker_Password')]) {
            sh 'docker login -u $Docker_User -p $Docker_Password'
              sh  'docker push sarvarayudu/javaapp:latest'
          }
		  

}
        
                  
          
  }
    }
	}
