pipeline {
	agent {label 'Jenkins-Agent'}
	tools {
		jdk 'Java17'
		maven 'Maven3'
	}
	stages{
		stage('Clean Workspace') {
		steps{
			cleanWs()
			}
		}
		stage('CheckOut From SCM') {
			steps{
			git branch: 'main', creadentialsId: 'github', url: 'https://github.com/Mubbaseer-Makrod/register-app.git'
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
	}
	
}
