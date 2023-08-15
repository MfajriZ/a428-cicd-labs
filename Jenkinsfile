pipeline {
    agent {
        docker {
            image 'node:16' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
         stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval'){
            steps{
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }
        stage('Deploy') {
	        steps {
		        script {
			        def remoteCommandsStart = """
				        cd /home/ubuntu/a428-cicd-labs/
				        git pull origin react-app
				        pm2 start npm --name "react-app" -- start
			        """
			        def remoteCommandsStop = """
				        pm2 stop react-app
			        """
			        sshagent(credentials: ['ubuntu']) {
				        sh "ssh -o StrictHostKeyChecking=no ubuntu@ec2-18-140-55-135.ap-southeast-1.compute.amazonaws.com '''${remoteCommandsStart}'''"
				        sleep(time: 1, unit: 'MINUTES')
				        sh "ssh -o StrictHostKeyChecking=no ubuntu@ec2-18-140-55-135.ap-southeast-1.compute.amazonaws.com '''${remoteCommandsStop}'''"
			        }
		        }
	        }
        }
    }
}