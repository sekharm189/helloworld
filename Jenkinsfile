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
		gitPassword = '2c83a81c0d2d53156fcb82de7268e6ffa6ab74f8'
		GITHUB_TOKEN = credentials('github-03')  	
		
	}    
	tools {
		maven 'package-builder-maven-3.3.9'
		jdk 'jdk-1.8.0_121'
	}
	stages {
		/*stage('Git Branch creation') {      
			steps {
				sh "git checkout -b ${branch}"
				sh "git branch"
				sh "git push  https://${gitUser}:${gitPassword}@github.com/${gitUser}/${gitProject}.git HEAD:${branch} -f"
			}     
		}*/
		stage('Build') {   
			steps {
				sh "git checkout -b ${branch}"
				sh "git branch"
				sh "echo 'This is the release dummy version file release-1.0.${BUILD_NUMBER}' > release.md"
				sh "git add release.md"
				sh "git commit -m 'added dummy release version file'"
				sh "git push  https://${gitUser}:${gitPassword}@github.com/${gitUser}/${gitProject}.git HEAD:${branch} -f"
				sh "mvn -f pom.xml clean compile install -U -DbuildNumber=${BUILD_NUMBER} -DskipTests=true"
			
			}
		}
		stage('Unit Test') {      
			steps {
				sh 'mvn -f pom.xml test'
				sh 'git branch'
				sh 'git add target/surefire-reports/* -f'
				sh "git commit -m 'Fix broken email address'"
				sh 'git checkout master'
				sh "git merge ${branch}"
				sh "git push  https://${gitUser}:${gitPassword}@github.com/${gitUser}/${gitProject}.git HEAD:master -f"
				
			}     
		}
		
	}
	post {
		always {
			//cleanWs()
			echo 'This will run always'
		}
		success {
			echo 'This will run only if successful'
			cleanWs()
		}
		failure {
			echo 'Unit tests are failed so remote branch is deleted'
			sh "git push  https://${gitUser}:${gitPassword}@github.com/${gitUser}/${gitProject}.git ${branch} --delete"
			cleanWs()
		}
		unstable {
			echo 'This will run only if the run was marked as unstable'
		}
		changed {
			echo 'This will run only if the state of the Pipeline has changed'
			echo 'For example, if the Pipeline was previously failing but is now successful'
		}
	}
}
