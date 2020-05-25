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
				sh "mvn test"
			}
		}

		stage('Integration Test') {
			steps {
				sh "mvn failsafe:integration-test failsafe:verify"
			}
		}

		stage('Package') {
			steps {
				sh "mvn package -DskipTests"
			}
		}	

		stage("Build Docker Image") {
			steps {
				//docker build -t "mggray/currency-exchange-devops:$env.BUILD_TAG"
				script {
					dockerImage = docker.build("mggray/currency-exchange-devops:${env.BUILD_TAG}")
				}
			}
		}

		stage("Push Docker Image") {
			steps {
				script {
					docker.withRegistry('', 'dockerhub') {
						dockerImage.push();
						dockerImage.push('latest');
					}
				}
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