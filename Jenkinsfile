pipeline {
	agent any
	// agent { docker { image 'maven:3.6.3'} }
	environment {
		dockerHome = tool 'myDocker'
		mavenHome = tool 'myMaven'
		PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
	}
	stages {
		stage('Checkout'){
			steps {
				sh 'mvn --version'
				sh 'docker --version'
				echo "Build"
				echo "PATH - $PATH"
				echo "BUILD_NUMBER - $env.BUILD_NUMBER"
				echo "$env.JOB_NAME"
				echo "$env.BUILD_ID"
				echo "$env.BUILD_TAG"
			}
		}
		stage('Compile') {
			steps {
				sh "mvn clean compile"
			}
		}
		stage('Test') {
			steps {
				echo "mvn test"
			}
		}
		stage('Integration Test'){
			steps {
				echo "mvn failsafe:integration-test failsafe:verify"
			}
		}
		stage('Package'){
			steps {
				echo "mvn package -DskipTests"
			}
		}
		stage('build docker image'){
			steps {
				script {
					docker.build("kkkkkk123/jenkin-devops-microservice:${env.BUILD_TAG}")
				}
				
			}

		}
		stage('push docker image'){
			steps {
				script {
					docker.withRegistry('','dockerhub') {
					dockerImage.push();
					dockerImage.push('latest');
					}
				}

			}

		}
	}
}


