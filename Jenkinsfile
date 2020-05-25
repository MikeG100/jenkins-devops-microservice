pipeline {
	agent any
	//agent { docker { image 'node:13.8'}}

	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}

	stages {
		stage("Checkout") {
			steps {
				//sh 'node --version'
				//sh 'mvn --version'
				sh 'mvn --version'
				sh 'docker version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BuildNumber - $env.BUILD_NUMBER"
				echo "BUILD ID - $env.BUILD_ID"
				echo "JOB_NAME - $env.JOB_NAME"
				echo "BUILD TAG - $env.BUILD_TAG"
				echo "BUILD URL - $env.BUILD_URL"
			}
		}

		stage("Build") {
			steps {
				sh "mvn clean compile"
			}
		}

		stage('Test') { 
			steps {
				sh "mv test"
			}
		}

		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}
	} 
	
	post {
		always {
			echo 'Im an awesome.  I run always'
		}
		success {
			echo 'I run when it is successful'
		}
		failure {
			echo 'I run when I fail'
		}
	}
}	
