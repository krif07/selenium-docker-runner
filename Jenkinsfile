pipeline{
	agent any
	stages{
		stage("Pull Latest Image"){
			steps{
				withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
					bat "docker login --username=${user} --password=${pass}"
					bat "docker pull krif07/selenium-docker:latest"
				}
			}
		}
		stage("Start Grid"){
			steps{
				bat "docker-compose up -d hub chrome firefox"
			}
		}
		stage("Run Test"){
			steps{
				bat "docker-compose up search-module-chrome search-module-firefox book-flight-module-chrome book-flight-module-firefox"
			}
		}
	}
	post{
		always{
			archiveArtifacts artifacts: 'output/**'
			bat "docker-compose down"
			bat "rm -rf output/"
		}
	}
}