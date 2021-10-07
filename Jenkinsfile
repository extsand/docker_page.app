pipeline {
    agent any
		// agent {
		// 	label: python
		// }
    triggers {
			pollSCM('* * * * *')
		}
    environment {
        PROJECT = "docker.page"
        DESCRIPTIONS = "GitHub -> Jenkins -> Docker -> -> AWS"
        OWNER = "extsand"
        GIT_REPO = "https://github.com/extsand/page.app.git"  
				DOCKER_HUB_IMAGE_NAME = "extsand/academy_docker.page:latest"
    } 

		options {
        buildDiscarder(logRotator(
					numToKeepStr: '3', 
					artifactNumToKeepStr: '3'
					))
        timestamps()
    }


    stages {
        stage('Introducting') {
            steps {
                echo "Hello User! \n
											It is ${PROJECT} project. \n
											We will use pipeline ${DESCRIPTIONS} . \n
											You can see files in ${GIT_REPO} ."
            }
        }

				stage('Clone Git Repository'){
					steps {
						echo "Get app files from ${GIT_REPO}"
						git branch: 'main', 
						credentialsId: 'extsand_git_credentials', 
						url: 'git@github.com:extsand/page.app.git'
					}
				}
				stage('DockerHub login'){
					steps{
						echo '============ Log in Docker Hub ===================='
						withCredentials(
							[usernamePassword(
									credentialsId: 'dockerhub_extsand', 
									passwordVariable: 'dockerhub_password', 
									usernameVariable: 'dockerhub_username')]) {
								// Login to dockerhub
								sh 'docker login -u $dockerhub_username -p $dockerhub_password '
						}
					}
				}

				stage('Create docker image'){
					steps {
						echo '=========== Start docker to build docker image ============'
						dir ('./docker.page'){
							// sh 'docker-compose build'
							// sh 'docker-compose up'	
							// sh 'docker build .'
							// add tag from docker-hub
							sh 'docker build -t $DOCKER_HUB_IMAGE_NAME . '
						}
					}
				}
				stage('Publish docker image'){
					steps {
						echo '===========Push Docker image to DockerHub ============'
						sh 'docker push $DOCKER_HUB_IMAGE_NAME'
					}
				}







    





    
    }
    
}