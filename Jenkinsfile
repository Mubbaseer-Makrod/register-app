pipeline {
	agent {label 'Jenkins-Agent'}
	tools {
		jdk 'Java17'
		maven 'Maven3'
	}
	environment {
		APP_NAME = "register-app-pipeline"
		RELEASE = "1.0.0"
		DOCKER_PASS = "dockerhub"
		DOCKER_USER = "mubbaseer"
		IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
		IMAGE_TAG = "${RELEASE}" + "${BUILD_NUMBER}"
	}
	stages{
		stage('Clean Workspace') {
			steps{
				cleanWs()
			}
		}
		stage('CheckOut From SCM') {
			steps{
				git branch: 'main', credentialsId: 'github', url: 'https://github.com/Mubbaseer-Makrod/register-app.git'
			}
		}
		stage("Build Application") {
			steps{
				sh 'mvn clean package'
			}
		}
		stage("Test Application") {
			steps{
				sh 'mvn test'
			}
		}
		stage("SonarQube Analysis") {
			steps {
				script {
					withSonarQubeEnv(credentialsId: 'Jenkins-Sonarqube-Token') {
						sh 'mvn sonar:sonar'
					}
				}
			}
		}
		stage("Quality Gate") {
            		steps {
              			script {
					waitForQualityGate abortPipeline: false, credentialsId: 'Jenkins-Sonarqube-Token'
				}
            			}
          	}
		stage("Build and push Docker Image") {
			steps {
				script {
					docker.withRegistry('', DOCKER_PASS) {
					docker_image = docker.build"$(DOCKER_IMAGE)"
				}
					docker.withRegistry('', DOCKER_PASS) {
						docker_image.push("$(IMAGE_TAG)")
						docker_image.push("latest")
					}
				}
				
			}
	}
	
}
