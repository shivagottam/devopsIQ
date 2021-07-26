pipeline {
    agent {
        node {
            label 'slave1'
        }
    }
    stages {
	stage("Interactive_Input") {
            steps {
                script {
                def userInput = input(
                 id: 'serverIP', message: 'Enter IP of website host', 
                 parameters: [
                 [$class: 'TextParameterDefinition', defaultValue: 'None', description: 'Enter IP of website host', name: 'ip']
                ])
                echo ("IP: "+userInput['ip'])
                }
            }
        }
        stage('SCM checkout'){
            steps {
		git "https://github.com/vistasunil/devopsIQ.git"
            }
	}
	stage('Remove dockers'){
	    steps {
		sh "if [ `sudo docker ps -a -q|wc -l` -gt 0 ]; then sudo docker rm -f \$(sudo docker ps -a -q);fi"
		}
	}
	stage('Build'){
	    steps {
    		sh "sudo docker build /home/ubuntu/jenkins/workspace/ansiblejob -t vistasunil/devopsdemo"
	   }
	}
	stage('Docker Push'){
		steps {
	            sh "cat /home/ubuntu/docker_login.txt |sudo docker login --username vistasunil --password-stdin"
                    sh "sudo docker push vistasunil/devopsdemo:latest"
	        }
	}
	stage('Configure servers and Deploy') {
            	steps {
                	sh 'ansible-playbook docker.yaml'
            	}
        }
	stage ('Testing'){
		steps {
			sh "sudo apt install python3-pip -y"
			sh "pip3 install selenium"
			sh "python3 sel.py"
		}
	}
    }
}