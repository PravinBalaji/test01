pipeline{
	agent any 
        parameters {
           	 choice(name: 'BranchName', choices:['main','branch01'], description: 'to refresh the list, go to configure, disable "this build has parameters", launch build (without parameters)to reload the list and stop it, then launch it again (with parameters)')
	}
	
	stages{
		
	        stage("Run Tests") {
			steps {
			    sh "echo SUCCESS on ${BranchName}"
			}
		}
	
		stage('Checkout') {
			steps{
				checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'd86de54a-8fbb-4506-b150-d18aecabbe73', url: 'https://github.com/PravinBalaji/test01.git']])
   		
			}
		}
		stage('Stop and Remove Docker container') {
                    steps {
                        script {
                            def containerId = sh(returnStdout: true, script: "docker ps -a | grep <container_name> | awk '{print $1}'").trim()
                            if (containerId) {
                                sh "docker stop ${containerId}"
                                sh "docker rm ${containerId}"
                            }
                        }
                    }
                }			
		stage("build docker image"){
		     	steps {
				
				sh '''
					docker build -t nginx/nodeapp_test:latest .
					docker run -d --name nginx1234 -p 8000:80 nginx/nodeapp_test:latest
				'''
			}
		}

		stage("docker container"){
			steps {
				echo'running docker image into container..'
			}
		}
	
	}
}
