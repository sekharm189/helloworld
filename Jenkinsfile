pipeline {
    agent {
	    node{
            label 'linux-64'
            customWorkspace "workspace/${env.JOB_NAME}"
            }
    }	

    environment {
        GITHUB_TOKEN = credentials('github-03')  	
	
    }    
	tools {
        maven 'package-builder-maven-3.3.9'
        jdk 'jdk-1.8.0_121'
	}
	
	
  
    stages {
	    stage('Git Branch creation') {      
        	   steps {
			   branch = build-${BUILD_NUMBER}
			   gitProject = helloworld
			   gitUser = sekharm189
			   gitPassword = Answer@12!
			   sh "git checkout -b ${branch}"
			   sh "git ${branch}"
			   sh "git push -u origin ${branch}"
			   sh "git push  https://${gitUser}:${gitPassword}@github.com/${gitProject} HEAD:${gitBranch} -f"
        }     
      }
	    stage('Unit Test') {      
        	   steps {
            		    sh 'mvn -f pom.xml test -DskipTests=true' 
        }     
      }
	    stage('Build') {   
		    steps {
			    sh "mvn -f pom.xml clean compile install -U -DbuildNumber=${BUILD_NUMBER} -DskipTests=true"
			    archiveArtifacts artifacts: 'target/*.jar'
		            
            }
        }
	  
        }
    post {
	    always{
         //   cleanWorkspace()
		 echo "clean"
        }
	   
	 /*  success {
            successEmail()
        }
        failure {
            failureEmail()
        }*/
    }
}
