pipeline { 
    environment { 
        registry = "shankarpatil1/assignment2" 
        registryCredential = 'docker-hub' 
        dockerImage = '' 
    }
    agent any 
    stages { 
        stage('Cloning Git') { 
            steps { 
                git (url: 'https://github.com/shanpatil/shanpatil.git', credentialsId: '63e7cf28-cc28-4865-8a15-2ab1e5a52237', branch:"main")
            }
        }
		stage('Build the code') { 
            steps { 
                script { 
                    sh 'mvn clean install'
                }
            } 
        }
        stage('Building image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Push image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        }
		stage('Pull image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.pull() 
                    }
                } 
            }
        }
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
}
