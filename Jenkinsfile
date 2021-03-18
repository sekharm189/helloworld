pipeline {
    agent {
	    node{
            label 'linux-64'
            customWorkspace "workspace/${env.JOB_NAME}"
            }
    }	

    environment {
	    branch = "build-${BUILD_NUMBER}"
	    gitProject = 'helloworld'
	    gitUser = 'sekharm189'
	    gitPassword = 'Answer_12'
	    GITHUB_TOKEN = credentials('github-03')  	
	
    }    
	tools {
        maven 'package-builder-maven-3.3.9'
        jdk 'jdk-1.8.0_121'
	}
	
	
  
    stages {
	    stage('Git Branch creation') {      
        	   steps {
			   
			   sh "git checkout -b ${branch}"
			   sh "git branch"
			   sh "git push  https://${gitUser}:${gitPassword}@github.com/${gitUser}/${gitProject}.git HEAD:${branch} -f"
			   //sh "git branch -d ${branch}"
			   
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
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'Unit tests are failed so remote branch is deleted'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
    /*post {
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
    } */

}
